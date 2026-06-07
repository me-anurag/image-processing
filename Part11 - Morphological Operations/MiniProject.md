# Mini Project — Shadow Remover

## Goal
Take a scanned or photographed document with uneven shadows/lighting.
Use morphological Top Hat to flatten the background and produce uniform text.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def remove_shadow(image_path):
    img  = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # Large SE: captures broad background variations (shadows)
    se       = cv2.getStructuringElement(cv2.MORPH_RECT, (51, 51))
    tophat   = cv2.morphologyEx(gray, cv2.MORPH_TOPHAT, se)

    # Add tophat back to brighten local features against background
    enhanced = cv2.add(gray, tophat)

    # Final threshold for clean text
    clahe   = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8,8))
    cleaned = clahe.apply(enhanced)
    _, final = cv2.threshold(cleaned, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(gray,    cmap='gray'); axes[0].set_title('Original with shadow'); axes[0].axis('off')
    axes[1].imshow(enhanced,cmap='gray'); axes[1].set_title('After Top Hat');        axes[1].axis('off')
    axes[2].imshow(final,   cmap='gray'); axes[2].set_title('Final clean document'); axes[2].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('shadow_removed.jpg', final)
    print("Saved: shadow_removed.jpg")

remove_shadow('shadowed_document.jpg')
```
## Extension Challenges
1. Automatically find optimal SE size based on image dimensions
2. Handle colour documents: process each channel separately
3. Combine with document unwarper from Part03 for a full scan pipeline
