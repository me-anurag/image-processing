# Mini Project — AR Poster

## Goal
Point a webcam at a flat poster/book cover. Detect it with feature matching + homography.
Overlay a custom image on top of it, perfectly aligned to the poster corners.

```python
import cv2
import numpy as np

def ar_overlay(reference_path, overlay_path):
    ref     = cv2.imread(reference_path)
    overlay = cv2.imread(overlay_path)
    h_r, w_r = ref.shape[:2]

    sift = cv2.SIFT_create(nfeatures=1000)
    kp_r, d_r = sift.detectAndCompute(cv2.cvtColor(ref, cv2.COLOR_BGR2GRAY), None)
    bf = cv2.BFMatcher()

    cap = cv2.VideoCapture(0)
    while True:
        ret, frame = cap.read()
        if not ret: break

        kp_f, d_f = sift.detectAndCompute(cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY), None)
        if d_f is None or len(kp_f) < 10:
            cv2.imshow('AR Poster', frame); cv2.waitKey(1); continue

        good = [m for m,n in bf.knnMatch(d_r, d_f, k=2) if m.distance < 0.75*n.distance]

        if len(good) >= 10:
            src = np.float32([kp_r[m.queryIdx].pt for m in good]).reshape(-1,1,2)
            dst = np.float32([kp_f[m.trainIdx].pt for m in good]).reshape(-1,1,2)
            H, mask = cv2.findHomography(src, dst, cv2.RANSAC, 5.0)

            if H is not None and mask.sum() > 8:
                ov_resized = cv2.resize(overlay, (w_r, h_r))
                warped_ov  = cv2.warpPerspective(ov_resized, H, (frame.shape[1], frame.shape[0]))
                mask_ov    = cv2.warpPerspective(np.ones((h_r,w_r),np.uint8)*255, H,
                                                  (frame.shape[1],frame.shape[0]))
                frame[mask_ov > 0] = warped_ov[mask_ov > 0]

                corners = np.float32([[0,0],[w_r-1,0],[w_r-1,h_r-1],[0,h_r-1]]).reshape(-1,1,2)
                sc = cv2.perspectiveTransform(corners, H)
                cv2.polylines(frame, [np.int32(sc)], True, (0,255,0), 2)

        cv2.imshow('AR Poster — press Q to quit', frame)
        if cv2.waitKey(1) & 0xFF == ord('q'): break

    cap.release(); cv2.destroyAllWindows()

ar_overlay('reference_poster.jpg', 'overlay_image.jpg')
```
