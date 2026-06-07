# Part 03 — Activities: Geometric Transformations

## Activity 1 — Resize Quality Comparison
Resize an image to 1/8 its size then back to original using all 4 interpolation methods. Display in a 2×2 grid. Write one sentence explaining which looks best and why.

## Activity 2 — Rotation Without Black Corners
Rotate an image 45°. Compute the largest centered rectangle that fits inside with no black borders. Crop to it. Display side by side with the original.

## Activity 3 — Book Flattener
Photograph any book or printed page at an angle with your phone. Identify the 4 page corners (you can hardcode pixel coordinates). Apply `warpPerspective` to produce a clean top-down view.

## Activity 4 — Translation Animation
In a `.py` file: slide an image from left to right across a black canvas using a `for` loop and `cv2.imshow`. The image should smoothly enter from the left edge and exit on the right.

## Activity 5 — Manual Affine Matrix
Without any helper function, build the 2×3 matrix that simultaneously: rotates 30°, scales by 0.75, and translates right by 80px. Apply and verify visually.
