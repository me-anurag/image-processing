# Mini Project — Ruler App

## Goal
Calibrate webcam once. Measure any object's size in real cm using a credit-card reference.

```python
import cv2
import numpy as np
import pickle

# After calibration, save: pickle.dump({'mtx':mtx,'dist':dist}, open('calibration.pkl','wb'))
# Load calibration
try:
    cal = pickle.load(open('calibration.pkl','rb'))
    mtx, dist = cal['mtx'], cal['dist']
except FileNotFoundError:
    print("Run calibration first with a checkerboard (see Lesson03)")
    print("Then save: pickle.dump({'mtx':mtx,'dist':dist}, open('calibration.pkl','wb'))")
    mtx = dist = None

if mtx is not None:
    cap = cv2.VideoCapture(0)
    CARD_WIDTH_CM = 8.56  # credit card width

    while True:
        ret, frame = cap.read()
        if not ret: break
        undistorted = cv2.undistort(frame, mtx, dist)
        cv2.putText(undistorted,'Place credit card in frame, press M to measure',
                    (10,30),cv2.FONT_HERSHEY_SIMPLEX,0.6,(0,255,255),2)
        cv2.imshow('Ruler App', undistorted)
        if cv2.waitKey(1)&0xFF==ord('q'): break
    cap.release(); cv2.destroyAllWindows()
```
