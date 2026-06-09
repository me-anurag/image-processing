# Part 24 — Activities: Template Matching

## Activity 1 — All 6 Methods
Apply all 6 template matching methods to the same image+template. Plot the result heatmaps.
For SQDIFF methods, the minimum is the match. For others, the maximum. Verify all find the same location.

## Activity 2 — Threshold Sensitivity
For TM_CCOEFF_NORMED, plot score vs threshold (0.5 to 0.99). Count matches at each threshold.
Find: below what threshold does your template match start giving false positives?

## Activity 3 — Multi-Scale Logo Finder
Take a logo (e.g. a brand icon). Resize it to 50%, 100%, 150%, 200%. For each size,
run multi-scale matching. Verify the same location is found regardless of template size.

## Activity 4 — Template Matching vs Feature Matching
Take a book cover photo. Apply template matching (same image crop as template).
Then rotate the query 30° and try again. Fails. Then try ORB feature matching on the rotated version.
Document the difference.

## Activity 5 — Real-Time Template Finder (in .py file)
Load a template image. Open webcam. Run matchTemplate every frame. Draw green box where template is found (score > 0.7). Test: hold the template in front of camera.
