# Part 27 — Activities: Stereo Vision and Depth

## Activity 1 — Epipolar Lines
Take a stereo image pair. Find matching feature points. Draw epipolar lines.
Verify: matching points lie on the same horizontal line after rectification.

## Activity 2 — numDisparities Sweep
Run StereoBM with numDisparities = 16, 48, 96, 128. Display all disparity maps.
What detail is recovered with higher numDisparities?

## Activity 3 — BlockSize Effect
Hold numDisparities constant. Sweep blockSize = 5, 11, 21, 41.
Smaller blockSize = more detail but more noise. Find the tradeoff.

## Activity 4 — SGBM vs BM Quality
Compare StereoBM and StereoSGBM on the same image pair. Time both.
Where does SGBM produce significantly better depth maps?

## Activity 5 — Depth Measurement
Measure a known object (30cm wide ruler) in the disparity map.
Using Q matrix (from stereoCalibrate), compute its real depth. Verify against a physical ruler.
