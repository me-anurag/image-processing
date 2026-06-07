# Part 10 — Activities: Edge Detection

## Activity 1 — Gradient Decomposition
Load any image. Compute Sobel X and Sobel Y separately. Display each.
Answer: what kinds of edges does X detect? What does Y detect?
Now combine into magnitude. Where do you see edges that weren't obvious in either X or Y alone?

## Activity 2 — Canny Threshold Live Tuner (in .py file)
Open a webcam or image. Add two trackbars: threshold1 (0–300) and threshold2 (0–300).
Apply Canny in real time and display edges. Find: what threshold pair gives the cleanest edges for your scene?

## Activity 3 — Pencil Sketch
Take any photo. Apply: Canny edges → invert → blend with grayscale using cv2.addWeighted.
The result should look like a pencil sketch.

## Activity 4 — Blur Detection
Write a function that takes an image path and returns "SHARP" or "BLURRY" using Laplacian variance.
Test it on: a clearly sharp photo, a clearly blurry photo, and 3 borderline cases.
Find the right variance threshold for your threshold.

## Activity 5 — Scharr vs Sobel on Diagonal
Take a photo with many diagonal lines (staircase, diagonal fence, angled roof).
Compare Sobel and Scharr on it. Zoom into the diagonals — Scharr should be more accurate.
