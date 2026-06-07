# Part 01 — Activities: Setup and Foundations

These activities are done inside the lesson notebooks themselves. Each one follows directly after the concept it reinforces.

---

## Activity 1 — Pixel Inspector
Load any photo of your choice. Print every pixel value in the top-left 10×10 region.
```python
import cv2
img = cv2.imread('your_photo.jpg')
print(img[0:10, 0:10])
```
**What to observe:** Each row is a pixel. Each pixel is [B, G, R]. Notice the range of values.

---

## Activity 2 — Channel Surgery
Load a color image. Set the entire Blue channel to 0. Then set it back. Then set Green to 0.
```python
img_copy = img.copy()
img_copy[:, :, 0] = 0   # kill blue
```
**What to observe:** Without blue, warm tones dominate. Without green, the image shifts drastically. This builds intuition about what each channel contributes.

---

## Activity 3 — Crop the Eyes
Load a face photo. Crop just the eye region using `img[y1:y2, x1:x2]`. Display the crop.
**Challenge:** Do it without hardcoding coordinates — compute them as fractions of image height/width.

---

## Activity 4 — Three Windows
Display the same image three times side by side in one matplotlib figure: once in color, once in grayscale, once with only the red channel.
```python
fig, axes = plt.subplots(1, 3, figsize=(15, 5))
```
**What to observe:** Grayscale averages channels. Red channel alone shows brightness wherever red dominates.

---

## Activity 5 — Array vs File Size
Load an image. Print `img.nbytes`. Save it as `.png` and `.jpg`. Use `os.path.getsize()` to print file sizes.
```python
import os
print("Array:", img.nbytes, "bytes")
print("PNG:", os.path.getsize('output.png'), "bytes")
print("JPG:", os.path.getsize('output.jpg'), "bytes")
```
**What to observe:** The array is always the same size. The file is compressed. PNG is lossless. JPG is lossy. This matters when saving processed images.
