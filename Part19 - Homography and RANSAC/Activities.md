# Part 19 — Activities: Homography and RANSAC

## Activity 1 — Verify Homography
Compute H between two views of the same flat surface. Manually project 5 known points through H.
Measure the distance between projected points and their actual locations in image 2.
This distance is the reprojection error. Target: < 3 pixels.

## Activity 2 — RANSAC Robustness Test
Take 20 good matches. Manually add 10 completely wrong matches (random point pairs).
Run findHomography with and without RANSAC. Compare inlier counts and H quality.

## Activity 3 — AR Marker
Print a simple image (logo, symbol) and photograph it in your hand or on a table.
Use feature matching + homography to detect it and overlay a different image on top of it.

## Activity 4 — Align Two Photos
Take two photos of the same flat document/poster from slightly different positions.
Use feature matching + homography to align them perfectly. Check alignment by blending 50/50.

## Activity 5 — Homography Stress Test
Use the same scene photographed from: 10° tilt, 30° tilt, 45° tilt, 60° tilt.
At what angle does the homography start failing (inlier ratio drops below 30%)?
