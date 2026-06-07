# Part 09 — Activities: Noise and Restoration

## Activity 1 — Noise Type Identification
Take 3 images with unknown noise (find online or add artificially). Look at the histogram and zoom into pixel-level patterns. Identify: Gaussian, salt-and-pepper, or speckle? Then apply the correct filter and verify.

## Activity 2 — NLM h-value Sweep
Apply NLM to a noisy image with h = 3, 7, 10, 15, 25. Display all results. Find: below which h does noise remain? Above which h does the image look plasticky/over-smoothed?

## Activity 3 — Inpainting a Watermark
Take any image with a visible logo or text overlay. Create a mask by thresholding the watermark color. Apply both TELEA and NS inpainting. Compare quality, especially near edges.

## Activity 4 — Scratch Remover
Draw 5 white diagonal lines on an image (simulating scratches on an old photo). Create a mask from the lines. Inpaint. How clean is the result? What determines quality?

## Activity 5 — Motion Blur Direction
Apply motion blur at 0°, 30°, 45°, 90°. Then apply Wiener deconvolution with the correct kernel for each. Display all 4 restoration results. Does diagonal blur restore as cleanly as horizontal?
