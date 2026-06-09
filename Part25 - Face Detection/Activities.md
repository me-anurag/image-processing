# Part 25 — Activities: Face Detection

## Activity 1 — Haar Parameter Sensitivity
Run Haar detection with: scaleFactor=[1.05,1.1,1.2,1.4] × minNeighbors=[3,5,8,12].
That's 16 combinations. Plot a grid showing number of detections at each combo.

## Activity 2 — Haar Failure Cases
Collect 5 images where you expect Haar to fail: profile face, tilted 45°, partially occluded, far away, in low light. Run detection. Document: what % of faces are missed?

## Activity 3 — DNN Confidence Sweep
Run DNN detector with confidence threshold 0.2, 0.5, 0.7, 0.9. Count detections at each.
Find the threshold that balances detection rate vs false positive rate for your images.

## Activity 4 — Auto Face Blurring
Take any group photo or video. Detect all faces with DNN. Apply Gaussian blur to each face ROI.
Save anonymized output. This is a real GDPR compliance tool.

## Activity 5 — Real-Time Face Counter (in .py file)
Webcam feed. Count and display the number of faces in the current frame live.
Use DNN detector with confidence > 0.6. Handle: 0 faces, 1 face, multiple faces.
