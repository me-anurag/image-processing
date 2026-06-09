# Part 12 — Activities: Contours

## Activity 1 — Contour Count on Real Image
Take a photo of scattered coins or bolts on a flat surface. Threshold it. Find contours. Filter by area. Count objects. How accurate is it? What causes false detections?

## Activity 2 — Area Histogram
Find all contours in a thresholded image. Plot a histogram of their areas. This tells you the natural size distribution of objects and helps you pick a good area filter threshold.

## Activity 3 — Hull vs Contour Overlay
For the 3 largest contours in an image, draw both the exact contour (green) and its convex hull (red) on the same image. Measure the "convexity" = contour area / hull area for each.

## Activity 4 — Approximation for Shape Detection
Find a real image with rectangles (book covers, whiteboards, screens). Use approxPolyDP with epsilon=0.02. Filter for contours with exactly 4 points. Draw bounding boxes around them.

## Activity 5 — Nested Contours
Find an image with objects containing holes (letters like O, B, D, or donuts, rings).
Use RETR_CCOMP. Count: how many contours are "holes" (parent != -1)?
