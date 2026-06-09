# Part 32 — Activities: Computational Photography

## Activity 1 — Pyramid Reconstruction
Build a 5-level Gaussian pyramid. Build the Laplacian pyramid from it.
Reconstruct the original by collapsing the Laplacian pyramid. Compute the pixel difference.
It should be near zero — perfect reconstruction.

## Activity 2 — Pyramid Blending
Blend an apple and orange using Laplacian pyramid blending.
Compare to naive alpha blend at the seam. Pyramid should be seamless.

## Activity 3 — Seamless Clone Positions
Clone one object into a scene. Place it at 3 different positions: good background, bad background, near edge.
How does background similarity affect seamless cloning quality?

## Activity 4 — Dehaze Comparison
Find 3 foggy/hazy photos online. Apply dark channel prior dehazing to each.
For which scene type does it work best? (Answer: outdoor scenes with clear sky work best.)

## Activity 5 — Low Light Enhancement Pipeline
Take a very dark image. Compare: (1) simple brightness boost, (2) gamma correction,
(3) CLAHE, (4) Retinex-inspired: divide by local mean. Rank by quality.
