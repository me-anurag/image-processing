# Mini Project — Pencil Sketch Effect

## Goal
Convert any photo into a realistic pencil sketch using edge detection and blending.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def pencil_sketch(image_path, edge_sigma=1.5, blend=0.7):
    img  = cv2.imread(image_path)
    if img is None: print("Cannot load"); return

    gray    = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    blurred = cv2.GaussianBlur(gray, (0,0), edge_sigma)
    edges   = cv2.Canny(blurred, 30, 100)

    # Invert edges: black lines on white background
    edges_inv = cv2.bitwise_not(edges)

    # Blend with grayscale for pencil texture
    sketch = cv2.addWeighted(gray, blend, edges_inv, 1-blend, 0)

    # Color version: blend color image with sketch
    gray3   = cv2.cvtColor(sketch, cv2.COLOR_GRAY2BGR)
    colored_sketch = cv2.addWeighted(img, 0.3, gray3, 0.7, 0)

    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(cv2.cvtColor(img,             cv2.COLOR_BGR2RGB)); axes[0].set_title('Original');       axes[0].axis('off')
    axes[1].imshow(sketch, cmap='gray');                               axes[1].set_title('Pencil Sketch');   axes[1].axis('off')
    axes[2].imshow(cv2.cvtColor(colored_sketch,  cv2.COLOR_BGR2RGB)); axes[2].set_title('Color Sketch');    axes[2].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('pencil_sketch.jpg', sketch)
    cv2.imwrite('color_sketch.jpg',  colored_sketch)
    print("Saved: pencil_sketch.jpg and color_sketch.jpg")

pencil_sketch('portrait.jpg')
```

## Extension Challenges
1. Add hatching effect: draw fine lines at 45° inside dark regions
2. Vary line thickness based on edge strength
3. Build a cartoon effect: Canny edges + K-means color segmentation combined
4. Process a video: apply sketch effect to every frame in real time
