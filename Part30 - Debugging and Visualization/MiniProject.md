# Mini Project — Pipeline Inspector Tool

## Goal
A reusable interactive tool: give it a list of (image, title) pairs, it displays them in a scrollable grid.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Button

def show_debug(images, titles=None, cols=4, figsize_per_img=(4,4)):
    n = len(images)
    if titles is None: titles=[f'Step {i}' for i in range(n)]
    rows = (n+cols-1)//cols
    fig, axes = plt.subplots(rows, cols, figsize=(figsize_per_img[0]*cols, figsize_per_img[1]*rows))
    if rows==1 and cols==1: axes=np.array([[axes]])
    elif rows==1: axes=axes.reshape(1,-1)
    elif cols==1: axes=axes.reshape(-1,1)

    for i,(img,title) in enumerate(zip(images,titles)):
        ax = axes[i//cols, i%cols]
        if len(img.shape)==2:
            ax.imshow(img, cmap='gray')
        else:
            ax.imshow(cv2.cvtColor(img,cv2.COLOR_BGR2RGB))
        ax.set_title(title, fontsize=10); ax.axis('off')

    for i in range(n, rows*cols):
        axes[i//cols,i%cols].axis('off')

    plt.tight_layout(); plt.show()

# Demo
img = cv2.imread('sample.jpg') if __import__('os').path.exists('sample.jpg') else np.random.randint(0,255,(300,400,3),dtype=np.uint8)
stages = [
    (img,                                                    'Original'),
    (cv2.cvtColor(img,cv2.COLOR_BGR2GRAY),                   'Grayscale'),
    (cv2.GaussianBlur(img,(15,15),0),                        'Blurred'),
    (cv2.Canny(cv2.cvtColor(img,cv2.COLOR_BGR2GRAY),50,150), 'Edges'),
]
images, titles = zip(*stages)
show_debug(list(images), list(titles), cols=4)
print("show_debug() is now your permanent debugging tool.")
print("Import it in every future project.")
```
