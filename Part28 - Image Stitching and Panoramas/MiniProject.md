# Mini Project — 360 Room Scanner

## Goal
Take 8 photos turning 45° each time. Stitch into a full 360° room panorama.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def stitch_panorama(image_paths):
    images = [cv2.imread(p) for p in image_paths]
    images = [img for img in images if img is not None]
    if len(images) < 2: print("Need at least 2 images"); return

    stitcher = cv2.Stitcher_create(cv2.Stitcher_PANORAMA)
    status, pano = stitcher.stitch(images)

    if status == cv2.Stitcher_OK:
        plt.figure(figsize=(20,6))
        plt.imshow(cv2.cvtColor(pano,cv2.COLOR_BGR2RGB))
        plt.title(f'Panorama from {len(images)} images'); plt.axis('off'); plt.show()
        cv2.imwrite('panorama.jpg', pano)
        print(f"Saved: panorama.jpg  ({pano.shape[1]}x{pano.shape[0]}px)")
    else:
        codes={cv2.Stitcher_ERR_NEED_MORE_IMGS:'Need more images',
               cv2.Stitcher_ERR_HOMOGRAPHY_EST_FAIL:'Homography failed — not enough overlap',
               cv2.Stitcher_ERR_CAMERA_PARAMS_ADJUST_FAIL:'Camera param adjustment failed'}
        print(f"Stitching failed: {codes.get(status,'Unknown error')}")
        print("Tips: ensure 30-50% overlap between consecutive images")

# Usage:
# stitch_panorama(['img1.jpg','img2.jpg','img3.jpg','img4.jpg','img5.jpg','img6.jpg','img7.jpg','img8.jpg'])
print("Photograph same scene from 8 angles (45° apart each), then call stitch_panorama()")
```
