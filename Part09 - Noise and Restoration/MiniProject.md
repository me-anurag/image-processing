# Mini Project — Object Remover

## Goal
Take any photo with an unwanted object (watermark, a person in the background, text overlay).
Mask the object and use inpainting to fill it naturally.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def remove_object(image_path, mask_path=None):
    img  = cv2.imread(image_path)
    if img is None: print("Cannot load image"); return

    if mask_path:
        mask = cv2.imread(mask_path, 0)
    else:
        # Interactive masking: user draws with mouse
        mask    = np.zeros(img.shape[:2], dtype=np.uint8)
        drawing = [False]
        display = img.copy()

        def draw_mask(event, x, y, flags, param):
            if event == cv2.EVENT_LBUTTONDOWN:
                drawing[0] = True
            elif event == cv2.EVENT_MOUSEMOVE and drawing[0]:
                cv2.circle(mask,    (x,y), 15, 255, -1)
                cv2.circle(display, (x,y), 15, (0,0,255), -1)
                cv2.imshow('Draw mask (left-click drag). Press SPACE when done.', display)
            elif event == cv2.EVENT_LBUTTONUP:
                drawing[0] = False

        cv2.namedWindow('Draw mask (left-click drag). Press SPACE when done.')
        cv2.setMouseCallback('Draw mask (left-click drag). Press SPACE when done.', draw_mask)
        cv2.imshow('Draw mask (left-click drag). Press SPACE when done.', display)
        while True:
            if cv2.waitKey(1) & 0xFF == ord(' '): break
        cv2.destroyAllWindows()

    # Dilate mask slightly to catch edges
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5,5))
    mask   = cv2.dilate(mask, kernel, iterations=2)

    # Inpaint
    result = cv2.inpaint(img, mask, inpaintRadius=7, flags=cv2.INPAINT_TELEA)

    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(cv2.cvtColor(img,    cv2.COLOR_BGR2RGB)); axes[0].set_title('Original');     axes[0].axis('off')
    axes[1].imshow(mask, cmap='gray');                       axes[1].set_title('Mask');          axes[1].axis('off')
    axes[2].imshow(cv2.cvtColor(result, cv2.COLOR_BGR2RGB)); axes[2].set_title('Object Removed'); axes[2].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('object_removed.jpg', result)
    print("Saved: object_removed.jpg")

remove_object('your_photo.jpg')
```

## Extension Challenges
1. Auto-detect the watermark by its color or transparency channel
2. Apply NLM denoising to the inpainted region to blend texture
3. Build a before/after slider comparison
4. Try on a video: inpaint the same region in every frame
