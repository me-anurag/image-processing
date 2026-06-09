# Part 31 — Activities: Performance and Optimization

## Activity 1 — Profile a Real Pipeline
Take a 5-step pipeline (gray → blur → threshold → contours → draw).
Wrap each step with time.time(). Run 100 frames. Print % time per step.
You will almost always be surprised by where the time goes.

## Activity 2 — ROI Speedup
Process full 1080p frame vs center 50% ROI. Measure FPS for each.
Calculate: what is the theoretical speedup? What is the actual speedup?

## Activity 3 — NumPy vs Loop
Write pixel-wise brightness boost two ways: (1) Python loop, (2) NumPy vectorized.
Run on a 1000x1000 image 10 times each. Record times. The ratio should be 100-500x.

## Activity 4 — Frame Skip
Process every 1st, 2nd, 3rd, 5th frame on a webcam. Measure FPS at each skip rate.
Plot: FPS vs skip_rate. What visual quality do you lose at each?

## Activity 5 — Threading Benchmark
Build two versions of a webcam loop: (1) single-threaded, (2) threaded capture.
Measure FPS on each running a moderately heavy pipeline. Document the improvement.
