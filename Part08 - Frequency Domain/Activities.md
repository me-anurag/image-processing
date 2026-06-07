# Part 08 — Activities: Frequency Domain

## Activity 1 — Spectrum Explorer
Load any image. Compute its DFT. Display the magnitude spectrum (log scale). Find: where are edges represented? Where is the sky/background? Zoom into the center.

## Activity 2 — Low-Pass Radius Sweep
Apply circular low-pass filters with radius = 5, 20, 50, 100, 200. Display all 5 results. Find the radius below which you start losing structural information.

## Activity 3 — Remove Stripes
Create a striped image (add sine wave to rows). View its spectrum — you'll see bright dots. Zero out those exact frequency locations. Verify the stripes are gone in the output.

## Activity 4 — Frequency vs Spatial
Apply Gaussian blur (σ=5) spatially. Apply a low-pass filter in frequency domain with equivalent radius. Display both. They should look almost identical — this is the duality principle.

## Activity 5 — JPEG Artifact Simulation
Write a DCT compression function. Apply at quality levels 100%, 50%, 20%, 5%. At what level do the 8×8 block artifacts become clearly visible?
