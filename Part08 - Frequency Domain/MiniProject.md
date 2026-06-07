# Mini Project — Fingerprint Enhancer

## Goal
Take a low-quality, noisy fingerprint image and use frequency domain filtering
to enhance ridge patterns and suppress background noise.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def enhance_fingerprint(image_path):
    img  = cv2.imread(image_path, 0)  # grayscale
    if img is None:
        # Generate synthetic fingerprint-like pattern for demo
        img = np.zeros((300,300), dtype=np.uint8)
        for i in range(300):
            img[i,:] = int(127 + 100*np.sin(i/5.0))
        img = cv2.GaussianBlur(img, (3,3), 0)
        noise = np.random.normal(0,30,img.shape).astype(np.int16)
        img   = np.clip(img.astype(np.int16)+noise, 0, 255).astype(np.uint8)

    gray = img.astype(np.float32)
    dft         = cv2.dft(gray, flags=cv2.DFT_COMPLEX_OUTPUT)
    dft_shifted = np.fft.fftshift(dft)
    h, w = gray.shape
    cx, cy = w//2, h//2

    # Band-pass filter: keep ridge frequencies, remove DC and high-freq noise
    mask = np.zeros((h, w, 2), np.float32)
    cv2.circle(mask, (cx,cy), 80,  (1,1), -1)   # outer: keep up to radius 80
    cv2.circle(mask, (cx,cy), 5,   (0,0), -1)   # inner: remove DC component

    filtered = dft_shifted * mask
    back     = np.fft.ifftshift(filtered)
    enhanced = cv2.idft(back, flags=cv2.DFT_SCALE | cv2.DFT_REAL_OUTPUT)
    enhanced = cv2.normalize(enhanced, None, 0, 255, cv2.NORM_MINMAX).astype(np.uint8)

    # CLAHE on the enhanced result
    clahe    = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8,8))
    enhanced = clahe.apply(enhanced)

    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(img,      cmap='gray'); axes[0].set_title('Original fingerprint'); axes[0].axis('off')
    mag = np.log1p(cv2.magnitude(dft_shifted[:,:,0], dft_shifted[:,:,1]))
    axes[1].imshow(mag, cmap='hot');       axes[1].set_title('Frequency spectrum'); axes[1].axis('off')
    axes[2].imshow(enhanced, cmap='gray'); axes[2].set_title('Enhanced ridges');    axes[2].axis('off')
    plt.tight_layout(); plt.show()

    cv2.imwrite('fingerprint_enhanced.jpg', enhanced)

enhance_fingerprint('fingerprint.jpg')
```
