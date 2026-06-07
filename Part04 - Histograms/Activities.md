# Part 04 — Activities: Histograms

## Activity 1 — Diagnose Before Looking
Collect 3 photos: one clearly dark, one clearly bright, one well-lit. Plot each histogram. Write a one-sentence diagnosis per image based only on the histogram — before looking at the photo. Were you right?

## Activity 2 — Live Histogram (in .py file)
Open a webcam feed. Show the live frame on the left and a real-time grayscale histogram on the right in the same window, updating every frame. Wave the camera between dark and bright areas and watch the histogram shift.

## Activity 3 — CLAHE vs Global on Shadows
Find or create a photo with strong shadows (half dark, half bright). Apply global equalization and CLAHE. Compare side by side. Write why global equalization fails on this image but CLAHE doesn't.

## Activity 4 — clipLimit Sweet Spot
Apply CLAHE to a portrait with clipLimit = 0.5, 2.0, 5.0, 10.0, 20.0. Display all five. Identify the point where it starts looking artificial (haloing, noise amplification).

## Activity 5 — Backprojection Color Finder
Take any photo with a distinctive colored object (orange, bright red, etc.). Define the object as ROI. Run backprojection on a second, different photo. Does it find regions of the same color?
