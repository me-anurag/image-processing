# Part 30 — Activities: Debugging and Visualization

## Activity 1 — Break It 3 Ways
Take a working segmentation pipeline. Break it in 3 ways:
1. Don't convert to grayscale before thresholding
2. Use wrong threshold value
3. Skip morphological cleanup
For each: inspect intermediate outputs to diagnose the problem. Practice the debug workflow.

## Activity 2 — Histogram Diagnosis
Apply 5 different brightness values to the same image. Plot the histogram of the mask at each step.
Learn to read: what does a good mask histogram look like vs a bad one?

## Activity 3 — Build show_debug()
Write a utility function `show_debug(images, titles, cols=4)` that displays any number of images in a grid.
This is the single most reusable function you'll write all curriculum.

## Activity 4 — Pipeline Step Stepper (in .py file)
Build a tool that shows one pipeline step at a time. Press SPACE to advance to next step.
Each step shows: step name, intermediate image, and a one-line description.

## Activity 5 — Common Failures Gallery
Create 5 images showing classic failures: wrong colorspace, bad threshold, too much blur, no morphology, inverted mask.
For each, show the wrong result, the diagnosis, and the fix.
