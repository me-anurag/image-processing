# Part 28 — Activities: Image Stitching

## Activity 1 — Two-Image Stitch
Take two overlapping photos. Stitch manually: ORB→match→RANSAC→warpPerspective→place on canvas.
Compare with cv2.Stitcher_create() automated result.

## Activity 2 — Canvas Size Computation
After finding homography H, compute where all 4 corners of image1 project into image2's space.
The bounding box of all projected corners + image2 gives you the canvas size.

## Activity 3 — Seam Comparison
Stitch two images. First: paste directly (hard seam visible). Second: add linear alpha blend in overlap.
Third: use Laplacian pyramid blending. Compare quality.

## Activity 4 — Exposure Mismatch
Take two photos of same scene with different exposures. Stitch them. See the brightness seam.
Then apply histogram matching before stitching. Seam should be less visible.

## Activity 5 — 4-Image Panorama
Take 4 overlapping photos rotating 30° each time. Chain 3 homographies.
Final output should cover 90°+ field of view.
