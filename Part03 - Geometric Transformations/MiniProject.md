# Mini Project — Document Unwarper

## Goal
Take any photo of a document, book, or whiteboard shot at an angle → output a clean, flat, perfectly rectangular scan.

## How It Works
1. Detect edges with Canny
2. Find contours, pick the largest 4-sided one
3. Order its 4 corners (top-left, top-right, bottom-right, bottom-left)
4. Compute output size from the corner distances
5. Apply `warpPerspective` to flatten

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def order_points(pts):
    rect = np.zeros((4, 2), dtype=np.float32)
    s    = pts.sum(axis=1)
    diff = np.diff(pts, axis=1)
    rect[0] = pts[np.argmin(s)]     # top-left     (smallest x+y)
    rect[2] = pts[np.argmax(s)]     # bottom-right (largest  x+y)
    rect[1] = pts[np.argmin(diff)]  # top-right    (smallest x-y)
    rect[3] = pts[np.argmax(diff)]  # bottom-left  (largest  x-y)
    return rect

def unwarp_document(image_path):
    img  = cv2.imread(image_path)
    orig = img.copy()

    gray  = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    blur  = cv2.GaussianBlur(gray, (5, 5), 0)
    edges = cv2.Canny(blur, 75, 200)

    contours, _ = cv2.findContours(edges, cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
    contours     = sorted(contours, key=cv2.contourArea, reverse=True)[:5]

    doc_cnt = None
    for c in contours:
        peri  = cv2.arcLength(c, True)
        approx = cv2.approxPolyDP(c, 0.02 * peri, True)
        if len(approx) == 4:
            doc_cnt = approx
            break

    if doc_cnt is None:
        print("Could not find 4-sided document contour.")
        return

    pts  = doc_cnt.reshape(4, 2).astype(np.float32)
    rect = order_points(pts)
    tl, tr, br, bl = rect

    maxW = int(max(np.linalg.norm(br - bl), np.linalg.norm(tr - tl)))
    maxH = int(max(np.linalg.norm(tr - br), np.linalg.norm(tl - bl)))

    dst = np.float32([[0,0],[maxW-1,0],[maxW-1,maxH-1],[0,maxH-1]])
    M   = cv2.getPerspectiveTransform(rect, dst)
    warped = cv2.warpPerspective(img, M, (maxW, maxH))

    fig, axes = plt.subplots(1, 2, figsize=(14, 6))
    axes[0].imshow(cv2.cvtColor(orig,   cv2.COLOR_BGR2RGB)); axes[0].set_title('Original Photo'); axes[0].axis('off')
    axes[1].imshow(cv2.cvtColor(warped, cv2.COLOR_BGR2RGB)); axes[1].set_title('Unwarped Scan');  axes[1].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('scanned_document.jpg', warped)
    print("Saved: scanned_document.jpg")

unwarp_document('document_photo.jpg')
```

## Extension Challenges
1. After warping, apply adaptive threshold to produce a print-ready black-and-white version
2. If auto-detection fails, allow the user to click 4 corners manually
3. Handle A4 vs letter — auto-detect orientation and pad to standard aspect ratio
4. Build a real-time version: webcam feed shows green outline when document is detected, press SPACE to capture and unwarp
