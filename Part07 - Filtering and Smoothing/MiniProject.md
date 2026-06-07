# Mini Project — Portrait Smoother

## Goal
Take a face photo. Apply bilateral filtering to smooth skin texture while keeping
all edges (eyes, lips, hair, glasses) sharp. Produce a professional before/after comparison.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def portrait_smoother(image_path, smoothing_strength='medium'):
    img = cv2.imread(image_path)
    if img is None: print("Cannot load image"); return

    params = {
        'light':  dict(d=9,  sigmaColor=40,  sigmaSpace=40),
        'medium': dict(d=15, sigmaColor=75,  sigmaSpace=75),
        'heavy':  dict(d=20, sigmaColor=120, sigmaSpace=120),
    }
    p = params[smoothing_strength]

    # Apply bilateral multiple times for stronger effect
    result = img.copy()
    for _ in range(3):
        result = cv2.bilateralFilter(result, p['d'], p['sigmaColor'], p['sigmaSpace'])

    # Side by side comparison
    comparison = np.hstack([img, result])
    h, w = img.shape[:2]
    cv2.line(comparison, (w, 0), (w, h), (255,255,255), 3)
    cv2.putText(comparison, 'BEFORE', (20, 40), cv2.FONT_HERSHEY_SIMPLEX, 1.2, (255,255,255), 2)
    cv2.putText(comparison, 'AFTER',  (w+20, 40), cv2.FONT_HERSHEY_SIMPLEX, 1.2, (255,255,255), 2)

    plt.figure(figsize=(16, 8))
    plt.imshow(cv2.cvtColor(comparison, cv2.COLOR_BGR2RGB))
    plt.title(f'Portrait Smoother — strength: {smoothing_strength}', fontsize=14)
    plt.axis('off'); plt.tight_layout(); plt.show()

    cv2.imwrite('portrait_smoothed.jpg', result)
    print("Saved: portrait_smoothed.jpg")

portrait_smoother('face_photo.jpg', smoothing_strength='medium')
```

## Extension Challenges
1. Detect skin pixels using HSV masking — only apply bilateral to skin, not to eyes/lips/hair
2. Add a frequency separation: smooth only low frequencies, keep high-frequency texture
3. Build a GUI with a strength slider using trackbars
4. Compare: bilateral 3× vs single heavy application — which looks more natural?
