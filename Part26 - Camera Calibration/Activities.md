# Part 26 — Activities: Camera Calibration

## Activity 1 — Print and Photograph Checkerboard
Print a 9x6 checkerboard. Photograph it 15 times at different angles and distances.
Run calibration. What is your reprojection error? Below 0.5px is excellent.

## Activity 2 — Reprojection Error vs Photo Count
Calibrate with 5, 10, 15, 20 photos. Plot reprojection error vs photo count.
At what count does adding more photos stop improving the calibration?

## Activity 3 — Distortion Visualization
Draw a grid of straight lines on a flat surface. Photograph it with a wide-angle lens.
Apply undistort. Lines should become straight. Visualize the warp field.

## Activity 4 — Real Measurement
Calibrate your camera. Place a standard A4 paper in frame.
Measure its pixel width. Compute real-world cm/pixel ratio. Measure 3 other objects and verify.

## Activity 5 — Phone vs Webcam Distortion
Calibrate both cameras. Compare their distortion coefficients.
Which has more radial distortion? Plot distortion maps for both.
