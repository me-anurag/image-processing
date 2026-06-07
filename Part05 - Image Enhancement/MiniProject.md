# Mini Project — Photo Enhancer Pipeline

## Goal
Take a raw, flat, unprocessed-looking photo and run it through a 5-stage enhancement pipeline. The output should look like a professionally edited image.

## The 5-Stage Pipeline

| Stage | Operation | Tool Used |
|---|---|---|
| 1 | Auto gamma from brightness | LUT |
| 2 | Local contrast | CLAHE on LAB L-channel |
| 3 | Color vibrancy | HSV saturation boost |
| 4 | Micro-detail recovery | Unsharp mask |
| 5 | Color grade | Per-channel LUT |

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def enhance_photo(image_path, debug=True):
    img = cv2.imread(image_path)
    if img is None: print("Cannot load image"); return

    stages = [('0 — Raw Input', img.copy())]

    # Stage 1: Auto Gamma
    mean  = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY).mean()
    gamma = max(0.4, min(1.8, 110.0 / (mean + 1e-6)))
    lut   = np.array([min(255, int((i/255.0)**(1.0/gamma)*255)) for i in range(256)], dtype=np.uint8)
    s1    = cv2.LUT(img, lut)
    stages.append((f'1 — Gamma {gamma:.2f}', s1.copy()))

    # Stage 2: CLAHE
    lab      = cv2.cvtColor(s1, cv2.COLOR_BGR2LAB)
    l, a, b  = cv2.split(lab)
    clahe    = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8,8))
    s2       = cv2.cvtColor(cv2.merge([clahe.apply(l), a, b]), cv2.COLOR_LAB2BGR)
    stages.append(('2 — CLAHE', s2.copy()))

    # Stage 3: Saturation
    hsv         = cv2.cvtColor(s2, cv2.COLOR_BGR2HSV).astype(np.float32)
    hsv[:,:,1]  = np.clip(hsv[:,:,1] * 1.3, 0, 255)
    s3          = cv2.cvtColor(hsv.astype(np.uint8), cv2.COLOR_HSV2BGR)
    stages.append(('3 — Saturation', s3.copy()))

    # Stage 4: Unsharp mask
    blur = cv2.GaussianBlur(s3, (0,0), 1.5)
    s4   = np.clip(s3.astype(np.float32)*1.4 - blur.astype(np.float32)*0.4, 0, 255).astype(np.uint8)
    stages.append(('4 — Sharpen', s4.copy()))

    # Stage 5: Warm LUT grade
    bc, gc, rc = cv2.split(s4)
    r_lut = np.array([min(255, int(i*1.05 + 5)) for i in range(256)], dtype=np.uint8)
    b_lut = np.array([max(0,   int(i*0.93))     for i in range(256)], dtype=np.uint8)
    s5 = cv2.merge([cv2.LUT(bc, b_lut), gc, cv2.LUT(rc, r_lut)])
    stages.append(('5 — Color Grade', s5.copy()))

    if debug:
        fig, axes = plt.subplots(2, 3, figsize=(19, 11))
        for ax, (name, im) in zip(axes.flat, stages):
            ax.imshow(cv2.cvtColor(im, cv2.COLOR_BGR2RGB))
            ax.set_title(name, fontsize=12); ax.axis('off')
        plt.suptitle('Photo Enhancer — 5-stage pipeline', fontsize=14)
        plt.tight_layout(); plt.show()

    cv2.imwrite('enhanced.jpg', s5)
    print(f"Saved enhanced.jpg  (input mean brightness was {mean:.0f})")
    return s5

enhance_photo('your_raw_photo.jpg')
```

## Extension Challenges
1. Add a `strength` parameter (0.0–1.0) that blends enhanced with original: `cv2.addWeighted(orig, 1-s, enhanced, s, 0)`
2. Process a whole folder, saving enhanced versions with `_enhanced` suffix
3. Add a `preset` parameter: 'portrait', 'landscape', 'street' — each uses different stage parameters
4. Build a before/after interactive slider using two `cv2.imshow` windows with a shared trackbar
