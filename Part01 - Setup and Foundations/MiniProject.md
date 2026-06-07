# Mini Project — Image Inspector

## Goal
Build a function that takes any image path and prints a complete, beautiful report about that image.

## What It Should Output
```
══════════════════════════════════════════
         IMAGE INSPECTOR REPORT
══════════════════════════════════════════
File      : sample.jpg
Dimensions: 1920 x 1080 px
Channels  : 3 (Color)
dtype     : uint8
Array RAM : 5,760.0 KB
──────────────────────────────────────────
Channel Stats (B / G / R):
  Min  :   0 /   0 /   0
  Max  : 255 / 251 / 248
  Mean :  87 / 102 / 114
──────────────────────────────────────────
Dominant color (avg): [87, 102, 114] → Bluish-grey
══════════════════════════════════════════
```

## Starter Code
```python
import cv2
import numpy as np
import os

def image_inspector(path):
    img = cv2.imread(path)
    if img is None:
        print("Error: Could not load image.")
        return

    h, w = img.shape[:2]
    channels = img.shape[2] if len(img.shape) == 3 else 1
    b, g, r = cv2.split(img) if channels == 3 else (img, img, img)

    print("═" * 42)
    print("         IMAGE INSPECTOR REPORT")
    print("═" * 42)
    print(f"File      : {os.path.basename(path)}")
    print(f"Dimensions: {w} x {h} px")
    print(f"Channels  : {channels} ({'Color' if channels == 3 else 'Grayscale'})")
    print(f"dtype     : {img.dtype}")
    print(f"Array RAM : {img.nbytes / 1024:.1f} KB")
    print("─" * 42)

    if channels == 3:
        print("Channel Stats (B / G / R):")
        print(f"  Min  : {b.min():3d} / {g.min():3d} / {r.min():3d}")
        print(f"  Max  : {b.max():3d} / {g.max():3d} / {r.max():3d}")
        print(f"  Mean : {int(b.mean()):3d} / {int(g.mean()):3d} / {int(r.mean()):3d}")

    avg = img.mean(axis=(0,1)).astype(int)
    print(f"Dominant color (avg): {avg.tolist()}")
    print("═" * 42)

# Run it
image_inspector('sample.jpg')
```

## Extension Challenges
1. Add a histogram bar (ASCII art) for each channel showing distribution
2. Detect if the image is overexposed (mean > 200) or underexposed (mean < 50) and print a warning
3. Accept a folder path and run the report on every image inside it
4. Add the actual file size from disk (not just array size) using `os.path.getsize()`
