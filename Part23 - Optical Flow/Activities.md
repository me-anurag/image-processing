# Part 23 — Activities: Optical Flow

## Activity 1 — Flow Colorwheel
Generate a color wheel image (hue = direction, saturation = distance from center).
This is the legend for your flow visualizations. Understand which color = which direction.

## Activity 2 — Motion Segmentation
Compute dense flow between two frames. Threshold the magnitude at different values (0.5, 1.5, 3.0).
Which threshold cleanly separates moving from static regions?

## Activity 3 — Trail Painter (in .py file)
Use Lucas-Kanade to track corner points on webcam. Draw trails of the last 20 positions.
Reinitialize lost points every 100 frames.

## Activity 4 — Speed Estimator
Track a moving object with LK flow. Compute average flow magnitude over its bounding box.
Calibrate: if you know the camera is 50cm from a surface, convert pixel/frame to cm/s.

## Activity 5 — Left vs Right Gesture
Webcam: detect dominant horizontal flow direction across the frame center strip.
Left-dominant flow = wave left. Right = wave right. Print direction in real time.
