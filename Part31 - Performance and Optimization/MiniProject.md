# Mini Project — FPS Optimizer

## Goal
Take a heavy pipeline at 3-4 FPS. Apply each optimization step by step. Target 25+ FPS.

```python
import cv2
import numpy as np
import time
from threading import Thread
from queue import Queue

class ThreadedCapture:
    def __init__(self, src=0):
        self.cap   = cv2.VideoCapture(src)
        self.queue = Queue(maxsize=2)
        self.stopped=False
        Thread(target=self._reader, daemon=True).start()
    def _reader(self):
        while not self.stopped:
            ret,frame=self.cap.read()
            if ret and not self.queue.full(): self.queue.put(frame)
    def read(self): return self.queue.get()
    def stop(self): self.stopped=True; self.cap.release()

def measure_fps(pipeline_fn, source=0, n_frames=100):
    vs  = ThreadedCapture(source)
    t0  = time.time()
    for _ in range(n_frames):
        frame = vs.read()
        pipeline_fn(frame)
    fps = n_frames / (time.time()-t0)
    vs.stop()
    return fps

# Pipeline v1: full resolution, every frame
def pipeline_v1(frame):
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray,(15,15),0)
    _,th = cv2.threshold(blur,0,255,cv2.THRESH_OTSU)
    cnts,_=cv2.findContours(th,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    return cnts

# Pipeline v2: half resolution
def pipeline_v2(frame):
    small=cv2.resize(frame,None,fx=0.5,fy=0.5)
    return pipeline_v1(small)

# Pipeline v3: half resolution + ROI only
def pipeline_v3(frame):
    h,w=frame.shape[:2]
    roi=frame[h//4:3*h//4, w//4:3*w//4]
    return pipeline_v2(roi)

print("Measuring FPS for each optimization level...")
print("Run this in a .py file with a webcam connected.")
print()")
print("Expected results:")
print("  v1 (full res):            ~5-8 FPS")
print("  v2 (half res):           ~15-20 FPS")
print("  v3 (half res + ROI):     ~25-35 FPS")
print("  v3 + threaded capture:   ~30-40 FPS")
```
