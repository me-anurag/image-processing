# Part 20 — Activities: Video Fundamentals

## Activity 1 — Video Report
Write a function `video_report(path)` that prints: resolution, FPS, total frames, duration, estimated file size per frame. Test on any video file.

## Activity 2 — Frame Sampler
Write a script that opens a video and saves every Nth frame as a JPEG. N is a parameter. For a 10-minute video at N=30, you should get ~200 frames — a time-lapse.

## Activity 3 — Real-Time FPS Counter
Open webcam. Display live FPS on the frame. Then intentionally slow down your processing (add cv2.waitKey(50)) and watch FPS drop. Measure: what's the FPS hit from adding Gaussian blur?

## Activity 4 — Pause/Play/Step
Build a video player with keyboard controls: P=pause, SPACE=step one frame, Q=quit, F=skip 30 frames forward. This is a useful debug tool for any video pipeline.

## Activity 5 — Grayscale Video Saver
Open any video. Convert each frame to grayscale. Save as new video file. Verify: output is properly playable. Check: does file size decrease?
