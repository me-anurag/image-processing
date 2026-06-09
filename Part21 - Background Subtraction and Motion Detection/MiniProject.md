# Mini Project — Intruder Alarm

## Goal
Webcam watches a static scene. The moment anything moves, it highlights the moving region,
prints "MOTION DETECTED", and saves a timestamped snapshot.

```python
import cv2
import numpy as np
from datetime import datetime
import os

os.makedirs('snapshots', exist_ok=True)

cap  = cv2.VideoCapture(0)
mog2 = cv2.createBackgroundSubtractorMOG2(history=500, varThreshold=50, detectShadows=False)
se   = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5,5))

alert_cooldown = 0

while True:
    ret, frame = cap.read()
    if not ret: break

    fg = mog2.apply(frame)
    fg = cv2.morphologyEx(fg, cv2.MORPH_OPEN,  se)
    fg = cv2.morphologyEx(fg, cv2.MORPH_CLOSE, se)

    contours, _ = cv2.findContours(fg, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    motion = [c for c in contours if cv2.contourArea(c) > 1500]

    vis = frame.copy()
    for c in motion:
        x,y,w,h = cv2.boundingRect(c)
        cv2.rectangle(vis, (x,y),(x+w,y+h),(0,0,255),2)

    if motion and alert_cooldown == 0:
        ts = datetime.now().strftime('%Y%m%d_%H%M%S_%f')
        cv2.imwrite(f'snapshots/{ts}.jpg', frame)
        print(f"MOTION DETECTED — snapshot saved: {ts}.jpg")
        alert_cooldown = 30  # don't save again for 30 frames

    if alert_cooldown > 0:
        cv2.putText(vis,'!! MOTION DETECTED !!',(10,40),
                    cv2.FONT_HERSHEY_SIMPLEX,1.2,(0,0,255),3)
        alert_cooldown -= 1

    cv2.imshow('Intruder Alarm — press Q to quit', vis)
    if cv2.waitKey(1) & 0xFF == ord('q'): break

cap.release(); cv2.destroyAllWindows()
```
