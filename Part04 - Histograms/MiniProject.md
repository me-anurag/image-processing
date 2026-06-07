# Mini Project — Auto Exposure Fixer

## Goal
Take any underexposed or overexposed photo, diagnose it automatically, and correct it to look properly exposed.

## Pipeline
1. Measure mean brightness
2. Diagnose: underexposed / overexposed / normal
3. Apply gamma correction if severely under/overexposed
4. Apply CLAHE for local contrast
5. Show before/after with histogram comparison

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def auto_fix_exposure(image_path):
    img = cv2.imread(image_path)
    if img is None:
        print("Cannot load:", image_path); return

    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    mean = gray.mean()

    # Diagnose
    if   mean < 60:  diagnosis, severity = "SEVERELY UNDEREXPOSED", "high"
    elif mean < 100: diagnosis, severity = "UNDEREXPOSED",           "medium"
    elif mean > 200: diagnosis, severity = "OVEREXPOSED",            "medium"
    elif mean > 230: diagnosis, severity = "SEVERELY OVEREXPOSED",   "high"
    else:            diagnosis, severity = "NORMAL EXPOSURE",         "none"
    print(f"Mean brightness: {mean:.1f} → {diagnosis}")

    result = img.copy()

    # Stage 1: Gamma for severe cases
    if severity == "high":
        if mean < 128:
            gamma = 2.2    # brighten
        else:
            gamma = 0.5    # darken
        lut = np.array([min(255, int((i/255.0)**(1.0/gamma)*255)) for i in range(256)], dtype=np.uint8)
        result = cv2.LUT(result, lut)

    # Stage 2: CLAHE
    lab = cv2.cvtColor(result, cv2.COLOR_BGR2LAB)
    l, a, b = cv2.split(lab)
    clip = 3.0 if severity != "none" else 1.5
    clahe = cv2.createCLAHE(clipLimit=clip, tileGridSize=(8,8))
    result = cv2.cvtColor(cv2.merge([clahe.apply(l), a, b]), cv2.COLOR_LAB2BGR)

    # Display
    fig, axes = plt.subplots(2, 2, figsize=(14, 10))
    axes[0,0].imshow(cv2.cvtColor(img,    cv2.COLOR_BGR2RGB)); axes[0,0].set_title(f'Before  (mean={mean:.0f})'); axes[0,0].axis('off')
    axes[0,1].imshow(cv2.cvtColor(result, cv2.COLOR_BGR2RGB)); axes[0,1].set_title('After — Auto Fixed');         axes[0,1].axis('off')
    axes[1,0].hist(gray.flatten(), 256, [0,256], color='steelblue'); axes[1,0].set_title('Before histogram')
    axes[1,1].hist(cv2.cvtColor(result, cv2.COLOR_BGR2GRAY).flatten(), 256, [0,256], color='steelblue'); axes[1,1].set_title('After histogram')
    plt.suptitle(f'Diagnosis: {diagnosis}', fontsize=13)
    plt.tight_layout(); plt.show()

    cv2.imwrite('fixed_exposure.jpg', result)
    print("Saved: fixed_exposure.jpg")

auto_fix_exposure('your_photo.jpg')
```

## Extension Challenges
1. Auto-compute gamma from the mean brightness (not hardcoded)
2. Fix white balance: detect color cast from per-channel means and correct it
3. Process a whole folder, producing a side-by-side PDF report
4. Add an interactive before/after slider
