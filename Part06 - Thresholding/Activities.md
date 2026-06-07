# Part 06 — Activities: Thresholding

## Activity 1 — All 5 Types Grid
Apply all 5 threshold types to the same image with threshold=127. Display in a 2×3 grid (original + 5 results). Write one use-case for each type in a markdown cell below.

## Activity 2 — Otsu on 3 Image Types
Apply Otsu to: a well-lit document, a low-contrast foggy photo, and a photo with strong shadows. Record the threshold Otsu chose for each. Where does it work? Where does it fail?

## Activity 3 — Make Otsu Fail, Then Fix
Take a low-contrast image where Otsu gives bad results. Apply CLAHE (clipLimit=3.0) first, then Otsu again. Display: original → bad Otsu → CLAHE+Otsu. See the rescue.

## Activity 4 — Notes Cleaner
Take a photo of handwritten notes with uneven lighting or shadows. Apply global Otsu vs adaptive Gaussian. Which produces cleaner, more readable text?

## Activity 5 — blockSize Experiment
Take a document photo with clear lighting variation. Apply adaptive Gaussian with blockSize = 7, 15, 31, 61. Display all four. Write: what does blockSize actually control perceptually?
