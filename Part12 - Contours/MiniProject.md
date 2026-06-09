# Mini Project — Shape Counter

## Goal
Take a photo of scattered objects on a flat surface (coins, bolts, buttons, seeds).
Count them accurately using contours. Display count on the image.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def count_objects(image_path, min_area=300, max_area=50000):
    img  = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray, (7,7), 0)

    # Adaptive threshold handles uneven lighting
    binary = cv2.adaptiveThreshold(blur, 255,
        cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY_INV, 11, 3)

    # Morphological cleanup
    se = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5,5))
    binary = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, se)
    binary = cv2.morphologyEx(binary, cv2.MORPH_OPEN,  se)

    contours, _ = cv2.findContours(binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    valid = [c for c in contours if min_area < cv2.contourArea(c) < max_area]

    vis = img.copy()
    for i, c in enumerate(valid):
        M  = cv2.moments(c)
        cx = int(M['m10']/(M['m00']+1e-6))
        cy = int(M['m01']/(M['m00']+1e-6))
        cv2.drawContours(vis, [c], -1, (0,255,0), 2)
        cv2.circle(vis, (cx,cy), 4, (0,0,255), -1)
        cv2.putText(vis, str(i+1), (cx-10,cy-10), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255,255,0), 2)

    cv2.putText(vis, f'Count: {len(valid)}', (10,40), cv2.FONT_HERSHEY_SIMPLEX, 1.5, (0,255,255), 3)

    fig, axes = plt.subplots(1,2, figsize=(16,7))
    axes[0].imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)); axes[0].set_title('Input'); axes[0].axis('off')
    axes[1].imshow(cv2.cvtColor(vis, cv2.COLOR_BGR2RGB)); axes[1].set_title(f'Counted: {len(valid)} objects'); axes[1].axis('off')
    plt.tight_layout(); plt.show()
    return len(valid)

n = count_objects('coins.jpg')
print(f"Final count: {n}")
```
