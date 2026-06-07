# Part 05 — Activities: Image Enhancement

## Activity 1 — Real-Time Brightness/Contrast Tuner
In a `.py` file: open a webcam feed. Add two OpenCV trackbars — one for alpha (0–30, representing 0.0–3.0) and one for beta (-100 to +100). Apply in real time. Explore the entire parameter space on a live face.

## Activity 2 — Gamma Curve Plot
For gamma values [0.3, 0.5, 0.7, 1.0, 1.5, 2.5] — plot the input→output mapping curve on one graph before applying to any image. This builds your mental model of what gamma actually does numerically.

## Activity 3 — Find the Right Sharpen
Take a deliberately blurry photo (or blur a sharp one with GaussianBlur sigma=5). Apply unsharp mask with strength values [0.5, 1.0, 2.0, 3.0, 5.0]. Find where halos start appearing. That's the practical limit.

## Activity 4 — Build Your Signature Grade
Design your own LUT-based color grade inspired by a film you love. Apply it to 3 different types of photos (portrait, landscape, street). Does it work universally or only on certain types?

## Activity 5 — Full Enhancement Chain
Write a function `enhance(img)` that chains: gamma → CLAHE → saturation boost → unsharp mask → LUT grade. Apply to 5 flat/raw-looking photos. Document what each stage contributes.
