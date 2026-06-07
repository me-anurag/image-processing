# Part 11 — Activities: Morphological Operations

## Activity 1 — SE Shape Zoo
Build and display all three SE shapes (RECT, ELLIPSE, CROSS) at sizes 5, 11, 21.
Apply each to the same binary image. Compare: how does shape affect the result?

## Activity 2 — Text Thickener/Thinner
Take a binary image of text (black text on white). Erode to thin the letters. Dilate to thicken them.
Find the maximum erosion before letters break. Find the maximum dilation before letters merge.

## Activity 3 — Noise vs Holes
Create a binary image with: (a) random small noise dots, and (b) random small holes.
Apply Opening to (a) and Closing to (b). Verify both are cleaned without changing main shapes.

## Activity 4 — Top Hat on Document
Photograph a printed document under a desk lamp that creates a gradient shadow across the page.
Apply morphological Top Hat to flatten the background and make all text uniformly dark.

## Activity 5 — Skeleton
Repeatedly erode a binary shape (letter, blob) and OR each iteration.
This produces the morphological skeleton. Do it 10 iterations and display each step.
