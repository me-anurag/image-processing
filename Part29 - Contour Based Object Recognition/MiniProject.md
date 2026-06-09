# Mini Project — Leaf Shape Classifier

## Goal
Photograph 3+ leaf types. Classify them by shape features without any deep learning.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def extract_shape_features(contour):
    M    = cv2.moments(contour)
    area = M['m00'] + 1e-6
    peri = cv2.arcLength(contour, True)
    x,y,w,h = cv2.boundingRect(contour)
    hull_area = cv2.contourArea(cv2.convexHull(contour)) + 1e-6

    return {
        'aspect_ratio': w/(h+1e-6),
        'solidity':     area/hull_area,
        'circularity':  4*np.pi*area/(peri**2+1e-6),
        'extent':       area/(w*h+1e-6),
        'hu':           cv2.HuMoments(M).flatten()
    }

def classify_leaf(features, rules):
    for name, rule in rules.items():
        if rule(features): return name
    return 'unknown'

# Define rules based on your leaf measurements
rules = {
    'round leaf':   lambda f: f['circularity'] > 0.5 and f['solidity'] > 0.85,
    'narrow leaf':  lambda f: f['aspect_ratio'] > 2.5,
    'lobed leaf':   lambda f: f['solidity'] < 0.75,
}

def process_leaf_photo(image_path):
    img  = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray,(5,5),0)
    _, binary = cv2.threshold(blur,0,255,cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)
    cnts,_ = cv2.findContours(binary,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    if not cnts: return
    c = max(cnts,key=cv2.contourArea)
    features = extract_shape_features(c)
    label    = classify_leaf(features, rules)
    vis = img.copy()
    cv2.drawContours(vis,[c],-1,(0,255,0),2)
    cv2.putText(vis,label,(20,50),cv2.FONT_HERSHEY_SIMPLEX,1.5,(0,255,0),3)
    plt.figure(figsize=(8,6))
    plt.imshow(cv2.cvtColor(vis,cv2.COLOR_BGR2RGB))
    plt.title(f'Classified as: {label}'); plt.axis('off'); plt.show()
    print(f"Features: {', '.join(f'{k}={v:.3f}' for k,v in features.items() if k!='hu')}")

process_leaf_photo('leaf.jpg')
```
