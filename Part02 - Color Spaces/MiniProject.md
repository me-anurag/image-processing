# Mini Project — Instagram Filter Builder

## Goal
Build 5 custom photo filters using color space manipulation. No lookup tables from the internet — derive each filter yourself from understanding the color spaces.

## The 5 Filters

### Filter 1: Warm (Golden Hour)
Boost reds and yellows, reduce blues.
```python
def warm_filter(img):
    # In BGR: boost R channel, reduce B channel
    result = img.copy().astype(np.float32)
    result[:,:,0] *= 0.8   # reduce Blue
    result[:,:,2] *= 1.2   # boost Red
    return np.clip(result, 0, 255).astype(np.uint8)
```

### Filter 2: Cold (Winter/Night)
Boost blues, reduce reds and yellows.
```python
def cold_filter(img):
    result = img.copy().astype(np.float32)
    result[:,:,0] *= 1.3   # boost Blue
    result[:,:,2] *= 0.8   # reduce Red
    return np.clip(result, 0, 255).astype(np.uint8)
```

### Filter 3: Vintage (Faded)
Reduce saturation in HSV, add a slight yellow tint, reduce contrast.
```python
def vintage_filter(img):
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV).astype(np.float32)
    hsv[:,:,1] *= 0.5     # reduce saturation by 50%
    result = cv2.cvtColor(hsv.astype(np.uint8), cv2.COLOR_HSV2BGR)
    result = (result * 0.8 + np.array([20, 30, 40])).clip(0,255).astype(np.uint8)
    return result
```

### Filter 4: Dramatic (High Contrast)
Boost contrast, slightly desaturate, darken shadows.
```python
def dramatic_filter(img):
    lab = cv2.cvtColor(img, cv2.COLOR_BGR2LAB).astype(np.float32)
    lab[:,:,0] = np.clip((lab[:,:,0] - 128) * 1.4 + 128, 0, 255)
    result = cv2.cvtColor(lab.astype(np.uint8), cv2.COLOR_LAB2BGR)
    hsv = cv2.cvtColor(result, cv2.COLOR_BGR2HSV).astype(np.float32)
    hsv[:,:,1] *= 0.8
    return cv2.cvtColor(hsv.astype(np.uint8), cv2.COLOR_HSV2BGR)
```

### Filter 5: Vivid (Oversaturated)
Massively boost saturation in HSV.
```python
def vivid_filter(img):
    hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV).astype(np.float32)
    hsv[:,:,1] = np.clip(hsv[:,:,1] * 1.8, 0, 255)
    return cv2.cvtColor(hsv.astype(np.uint8), cv2.COLOR_HSV2BGR)
```

## Display All Together
```python
filters = {
    'Original': img,
    'Warm': warm_filter(img),
    'Cold': cold_filter(img),
    'Vintage': vintage_filter(img),
    'Dramatic': dramatic_filter(img),
    'Vivid': vivid_filter(img),
}

fig, axes = plt.subplots(2, 3, figsize=(18, 10))
for ax, (name, im) in zip(axes.flat, filters.items()):
    ax.imshow(cv2.cvtColor(im, cv2.COLOR_BGR2RGB))
    ax.set_title(name, fontsize=14); ax.axis('off')
plt.tight_layout(); plt.show()
```

## Extension Challenges
1. Add a vignette (dark corners) effect to the vintage filter
2. Build a sixth filter: Black & White with high contrast (Ansel Adams style)
3. Allow filter intensity 0.0–1.0 — blend between original and full-filter
4. Save all 6 filtered versions to disk automatically
