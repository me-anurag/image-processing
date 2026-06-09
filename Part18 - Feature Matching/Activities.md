# Part 18 — Activities: Feature Matching

## Activity 1 — Ratio Test Threshold Sweep
Match SIFT features between two views of the same object. Apply ratio test with thresholds 0.5, 0.65, 0.75, 0.85, 0.95. Count good matches at each. Plot: threshold vs match count. What threshold gives the best quality-quantity balance?

## Activity 2 — Book Identifier
Photograph 5 different book covers. Store their ORB descriptors. Photograph any one again. Build a system that identifies which book by counting good matches to each stored set. The highest match count wins.

## Activity 3 — Match Quality Metric
For a known transformation (e.g., you applied a known rotation), check: what fraction of "good matches" correctly identify corresponding points? This is your match precision.

## Activity 4 — SIFT vs ORB on Blurred Image
Blur one of two matching images with sigma=5. Run SIFT matching and ORB matching. Which produces more good matches under blur? Which is more robust?

## Activity 5 — Match Under Illumination Change
Take two photos of the same scene: one with normal lighting, one with very different lighting (lamp on, lamp off). Run SIFT matching. How many good matches survive? What does this tell you about SIFT's illumination invariance?
