# Mini Project — Note Cleaner

## Goal
Take a photo of handwritten notes shot with a phone under imperfect lighting
(shadows, uneven brightness, slightly tilted). Output a clean, crisp black-and-white
version that looks like a scanned document — ready for printing or OCR.

## Pipeline
1. Load and optionally resize
2. Convert to grayscale
3. Apply CLAHE to even out lighting
4. Apply adaptive Gaussian threshold
5. Morphological cleanup (remove tiny noise dots)
6. Save clean output

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def clean_notes(image_path):
    img  = cv2.imread(image_path)
    orig = img.copy()

    # Resize if too large (speeds up processing)
    h, w = img.shape[:2]
    if w > 1500:
        scale = 1500 / w
        img   = cv2.resize(img, None, fx=scale, fy=scale, interpolation=cv2.INTER_AREA)

    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # CLAHE to handle uneven lighting
    clahe    = cv2.createCLAHE(clipLimit=2.5, tileGridSize=(8,8))
    enhanced = clahe.apply(gray)

    # Adaptive threshold
    clean = cv2.adaptiveThreshold(
        enhanced, 255,
        cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
        cv2.THRESH_BINARY,
        blockSize=11, C=4
    )

    # Morphological: remove tiny noise specks
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2,2))
    clean  = cv2.morphologyEx(clean, cv2.MORPH_OPEN, kernel)

    # Display
    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(cv2.cvtColor(orig, cv2.COLOR_BGR2RGB)); axes[0].set_title('Original Photo'); axes[0].axis('off')
    axes[1].imshow(enhanced, cmap='gray');                  axes[1].set_title('After CLAHE');    axes[1].axis('off')
    axes[2].imshow(clean,    cmap='gray');                  axes[2].set_title('Final — Clean Notes'); axes[2].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('clean_notes.png', clean)
    print("Saved: clean_notes.png")

clean_notes('your_notes_photo.jpg')
```

## Extension Challenges
1. Add deskewing: detect the text angle and rotate to make lines horizontal
2. Add a border crop: automatically remove the outer 2% which often has noise
3. Process multiple pages: loop over a folder, clean all, save as numbered outputs
4. Boost quality for OCR: after cleaning, test with pytesseract and measure word error rate
