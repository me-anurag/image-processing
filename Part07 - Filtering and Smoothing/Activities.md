# Part 07 — Activities: Filtering and Smoothing

## Activity 1 — Convolution from Scratch
Implement 2D convolution manually with NumPy loops on a 100×100 grayscale image.
Apply a 3×3 box kernel. Compare result to `cv2.filter2D`. Time both.
**Expected:** Identical results. Manual will be ~1000x slower — this is why libraries exist.

## Activity 2 — Noise Type vs Filter Type
Create two versions of the same image: one with Gaussian noise, one with salt-and-pepper noise.
Apply Gaussian blur and Median blur to each. That's 4 combinations.
**Question:** Which filter works on which noise? What happens when you use the wrong filter?

## Activity 3 — Sigma Dissolve
Create a 10-frame sequence: apply Gaussian blur with sigma = 1, 3, 5, 8, 12, 18, 25, 35, 50 to the same image.
Display all frames in a row. Watch the image dissolve. At what sigma does it become unrecognizable?

## Activity 4 — Portrait Smoother
Take a face photo. Apply bilateral filter (d=15, sigmaColor=75, sigmaSpace=75).
Zoom into the eye area in both original and result. Verify: skin is smoother, eye edges are sharp.

## Activity 5 — Custom Kernel Zoo
Build and apply these 5 kernels using `cv2.filter2D`:
- Identity (no change)
- Emboss (3×3 diagonal gradient)
- Motion blur (horizontal, 15×1 ones/15)
- Edge enhance
- Gaussian sharpen (DoG — difference of Gaussians)
Display all 5 results in a row.
