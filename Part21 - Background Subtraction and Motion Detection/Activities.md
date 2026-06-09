# Part 21 — Activities: Background Subtraction and Motion Detection

## Activity 1 — Differencing vs MOG2
On the same video, apply frame differencing and MOG2. Compare masks at frame 5, 20, 50.
Where does differencing fail that MOG2 handles correctly?

## Activity 2 — Learning Rate Explorer
Apply MOG2 with lr=0, 0.001, 0.01, 0.1 to a 100-frame video where lighting slowly changes.
Which learning rate best adapts to the light change while still detecting moving objects?

## Activity 3 — Shadow Handling
Enable `detectShadows=True` in MOG2. Run on a video with visible shadows.
Visualize: where are shadows (gray=127) vs true foreground (white=255)?
Mask out shadows in your final detection.

## Activity 4 — Intruder Alarm (in .py file)
Webcam version: detect the moment anything enters the frame. Print "MOTION DETECTED" to console
and save a timestamped JPEG snapshot. Use MOG2 + contour area threshold.

## Activity 5 — Morphology Tuning
Take a noisy foreground mask. Try OPEN with kernel 3, 7, 11 then CLOSE with kernel 5, 11, 21.
For each combination, count resulting contours. Find the kernel sizes that give the cleanest result.
