# Mini Project — Logo Detector

## Goal
Given a company logo as template, scan images and find every occurrence with its location.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def find_logo(template_path, scene_path, threshold=0.75):
    template = cv2.imread(template_path, 0)
    scene    = cv2.imread(scene_path)
    gray     = cv2.cvtColor(scene, cv2.COLOR_BGR2GRAY)
    tH, tW   = template.shape

    best_val, best_loc, best_scale = -1, None, 1.0
    for scale in np.linspace(0.3, 2.0, 25):
        resized = cv2.resize(gray, None, fx=scale, fy=scale)
        if resized.shape[0]<tH or resized.shape[1]<tW: continue
        res = cv2.matchTemplate(resized, template, cv2.TM_CCOEFF_NORMED)
        _, val, _, loc = cv2.minMaxLoc(res)
        if val > best_val: best_val,best_loc,best_scale=val,loc,scale

    vis = scene.copy()
    if best_val >= threshold:
        x=int(best_loc[0]/best_scale); y=int(best_loc[1]/best_scale)
        w=int(tW/best_scale);           h=int(tH/best_scale)
        cv2.rectangle(vis,(x,y),(x+w,y+h),(0,255,0),3)
        cv2.putText(vis,f'FOUND {best_val:.2f}',(x,y-10),cv2.FONT_HERSHEY_SIMPLEX,0.9,(0,255,0),2)
        print(f"Logo found at ({x},{y}) scale={best_scale:.2f} confidence={best_val:.3f}")
    else:
        print(f"Logo NOT found (best score={best_val:.3f} < threshold={threshold})")

    plt.figure(figsize=(10,6))
    plt.imshow(cv2.cvtColor(vis,cv2.COLOR_BGR2RGB)); plt.axis('off')
    plt.title(f'Logo detection result (threshold={threshold})'); plt.show()

find_logo('logo.jpg', 'scene.jpg')
```
