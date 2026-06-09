# Part 17 — Activities: Keypoints and Descriptors

## Activity 1 — Harris Breaks on Scale
Detect Harris corners on an image. Resize the image to 0.5x. Detect Harris again.
Do the same corners appear? Compare counts and locations.
Now repeat with SIFT. SIFT should find corresponding corners despite the scale change.

## Activity 2 — ORB vs SIFT Speed Benchmark
Run ORB and SIFT 50 times each on the same image. Record average time.
Plot a bar chart comparing: keypoints found, descriptor dimension, time per call.

## Activity 3 — Descriptor Distance
Extract SIFT descriptors from two very similar patches (same texture at different positions).
Extract from two very different patches. Compute L2 distance between all pairs.
Verify: similar patches → low distance. Different → high distance.

## Activity 4 — Scale Invariance Test
Take a photo. Extract ORB keypoints. Scale the image to 0.3x. Extract ORB again.
Using the known scale, check: which keypoints from the small image correspond to which in the large?
Do descriptors match even across a 3x scale change?

## Activity 5 — Keypoint Visualization Depth
For one SIFT keypoint: print its location (x,y), scale, orientation angle, and full 128-dim descriptor.
Visualize the 4x4 spatial grid (reshape descriptor to 8×16). Each column is one spatial cell.
