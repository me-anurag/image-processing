# Part 29 — Activities: Contour-Based Recognition

## Activity 1 — 4-Class Shape Classifier
Create binary images with circles, squares, triangles, and stars.
Extract: solidity, circularity, aspect ratio, Hu moments.
Build threshold rules (no ML) to classify each. Test on 20 new shapes.

## Activity 2 — Hu Moment Distance Matrix
Collect 5 shapes: circle, square, triangle, star, cross.
Compute all pairwise Hu moment distances. Build a 5x5 distance matrix.
Visualize as heatmap. Shapes in the same class should have low inter-distance.

## Activity 3 — Scale and Rotation Invariance Test
Take one shape. Resize to 3 different scales. Rotate at 5 different angles.
Compute Hu moments for all 15 variants. Plot: does the Hu distance stay low?

## Activity 4 — Leaf Classifier
Photograph 3-4 types of leaves. Extract shape features (aspect ratio, solidity, Hu).
Write classification rules. Test on held-out photos.

## Activity 5 — Playing Card Suit Recognition
Take photos of card suits (♥♦♣♠). Extract contour, compute shape features.
Build a 4-class classifier without any ML library.
