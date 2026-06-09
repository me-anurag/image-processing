# CAPSTONE — Smart Surveillance System

## This Is the Final Exam

Everything from Part01 through Part32 converges here. If you can build this, you are genuinely fluent in image processing. Not "used some OpenCV functions" fluent. Actually fluent.

---

## What It Does

A real-time webcam application that runs the complete classical CV pipeline:

| Feature | Technique Used From |
|---|---|
| Motion detection | Part21 — Background Subtraction (MOG2) |
| Object segmentation | Part12 — Contours on foreground mask |
| Object tracking | Part22 — Centroid tracker with persistent IDs |
| Face detection | Part25 — DNN face detector |
| Face anonymization | Part07 — Gaussian blur on face ROI |
| Tripwire counting | Part22 — Line crossing detection |
| Snapshot logging | Part20 — Frame saving with timestamp |
| Live dashboard | Part30 — Text overlay, debug display |
| Real-time FPS | Part31 — Threaded capture, optimization |

---

## Build Order

**Do not skip steps. Each one depends on the previous.**

### Step 1 — Motion Detection
Get MOG2 running. Verify you get a clean foreground mask. Add morphological cleanup (opening + dilation).

### Step 2 — Contour Detection
Find contours in the foreground mask. Filter by minimum area. Draw bounding boxes.

### Step 3 — Centroid Tracker
Assign each contour a persistent ID using centroid distance matching. IDs should survive small movements.

### Step 4 — Tripwire
Draw a horizontal line across the frame. Count when a tracked object's centroid crosses it.

### Step 5 — Face Detection
For each tracked object's bounding box ROI, run the DNN face detector. If a face is found, blur it.

### Step 6 — Snapshot Logging
When a new object ID appears, save a timestamped snapshot to disk.

### Step 7 — Dashboard Overlay
Display: FPS, active object count, total crossings, alert status — on the live frame.

### Step 8 — Threading
Move VideoCapture to a separate thread feeding a queue. Measure FPS improvement.

---

## Full Implementation

```python
# main.py — Smart Surveillance System
import cv2
import numpy as np
import time
import os
from datetime import datetime
from threading import Thread
from queue import Queue

# ── CONFIG ──────────────────────────────────────────────────────────────────
TRIPWIRE_Y = 0.5          # fraction of frame height for the tripwire line
MIN_CONTOUR_AREA = 800    # minimum area to consider a real object
FACE_MODEL_PATH = 'deploy.prototxt'
FACE_WEIGHTS_PATH = 'res10_300x300_ssd_iter_140000.caffemodel'
OUTPUT_DIR = 'snapshots'
os.makedirs(OUTPUT_DIR, exist_ok=True)

# ── THREADED CAPTURE ─────────────────────────────────────────────────────────
class VideoStream:
    def __init__(self, src=0):
        self.cap = cv2.VideoCapture(src)
        self.queue = Queue(maxsize=2)
        self.stopped = False
        Thread(target=self._read, daemon=True).start()

    def _read(self):
        while not self.stopped:
            ret, frame = self.cap.read()
            if ret:
                if not self.queue.full():
                    self.queue.put(frame)

    def read(self):
        return self.queue.get()

    def stop(self):
        self.stopped = True
        self.cap.release()

# ── CENTROID TRACKER ──────────────────────────────────────────────────────────
class CentroidTracker:
    def __init__(self, max_disappeared=30):
        self.next_id = 0
        self.objects = {}        # id -> centroid
        self.disappeared = {}    # id -> frames missing
        self.max_disappeared = max_disappeared

    def register(self, centroid):
        self.objects[self.next_id] = centroid
        self.disappeared[self.next_id] = 0
        self.next_id += 1

    def deregister(self, oid):
        del self.objects[oid]
        del self.disappeared[oid]

    def update(self, rects):
        if len(rects) == 0:
            for oid in list(self.disappeared.keys()):
                self.disappeared[oid] += 1
                if self.disappeared[oid] > self.max_disappeared:
                    self.deregister(oid)
            return self.objects

        input_centroids = np.array([[int((x+w)//2), int((y+h)//2)] for x,y,w,h in rects])

        if len(self.objects) == 0:
            for c in input_centroids:
                self.register(c)
        else:
            obj_ids = list(self.objects.keys())
            obj_centroids = np.array(list(self.objects.values()))

            D = np.linalg.norm(obj_centroids[:,None] - input_centroids[None,:], axis=2)
            rows = D.min(axis=1).argsort()
            cols = D.argmin(axis=1)[rows]

            used_rows, used_cols = set(), set()
            for r, c in zip(rows, cols):
                if r in used_rows or c in used_cols: continue
                if D[r,c] > 100: continue
                oid = obj_ids[r]
                self.objects[oid] = input_centroids[c]
                self.disappeared[oid] = 0
                used_rows.add(r); used_cols.add(c)

            unused_rows = set(range(len(obj_ids))) - used_rows
            unused_cols = set(range(len(input_centroids))) - used_cols

            for r in unused_rows:
                self.disappeared[obj_ids[r]] += 1
                if self.disappeared[obj_ids[r]] > self.max_disappeared:
                    self.deregister(obj_ids[r])

            for c in unused_cols:
                self.register(input_centroids[c])

        return self.objects

# ── MAIN LOOP ────────────────────────────────────────────────────────────────
def main():
    vs = VideoStream(0)
    time.sleep(1.0)  # let camera warm up

    bg_subtractor = cv2.createBackgroundSubtractorMOG2(
        history=500, varThreshold=50, detectShadows=False)
    tracker = CentroidTracker(max_disappeared=30)

    # Load face detector
    face_net = None
    if os.path.exists(FACE_MODEL_PATH) and os.path.exists(FACE_WEIGHTS_PATH):
        face_net = cv2.dnn.readNetFromCaffe(FACE_MODEL_PATH, FACE_WEIGHTS_PATH)
        print("Face detector loaded")
    else:
        print("Face model not found — face detection disabled")
        print("Download from: https://github.com/opencv/opencv/tree/master/samples/dnn/face_detector")

    tripwire_count = 0
    prev_centroids = {}
    logged_ids = set()
    fps_counter, fps_start = 0, time.time()
    fps = 0

    frame = vs.read()
    h_frame, w_frame = frame.shape[:2]
    tripwire_y = int(h_frame * TRIPWIRE_Y)

    while True:
        frame = vs.read()
        display = frame.copy()

        # ── Background subtraction ──
        fg_mask = bg_subtractor.apply(frame)
        kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (3,3))
        fg_mask = cv2.morphologyEx(fg_mask, cv2.MORPH_OPEN, kernel)
        fg_mask = cv2.dilate(fg_mask, kernel, iterations=2)

        # ── Contours ──
        contours, _ = cv2.findContours(fg_mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        rects = []
        for c in contours:
            if cv2.contourArea(c) < MIN_CONTOUR_AREA: continue
            x,y,w,h = cv2.boundingRect(c)
            rects.append((x,y,w,h))
            cv2.rectangle(display, (x,y), (x+w,y+h), (0,255,0), 2)

        # ── Tracking ──
        objects = tracker.update(rects)

        # ── Tripwire + snapshot ──
        for oid, centroid in objects.items():
            cx, cy = centroid

            # Tripwire crossing
            prev_cy = prev_centroids.get(oid, cy)
            if prev_cy < tripwire_y <= cy or cy < tripwire_y <= prev_cy:
                tripwire_count += 1
            prev_centroids[oid] = cy

            # Snapshot on first appearance
            if oid not in logged_ids:
                logged_ids.add(oid)
                ts = datetime.now().strftime('%Y%m%d_%H%M%S')
                cv2.imwrite(f'{OUTPUT_DIR}/obj_{oid}_{ts}.jpg', frame)

            # Draw centroid and ID
            cv2.circle(display, (cx,cy), 4, (255,0,0), -1)
            cv2.putText(display, f'ID {oid}', (cx-10, cy-10),
                cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,0), 1)

        # ── Face detection + blur ──
        if face_net is not None:
            blob = cv2.dnn.blobFromImage(cv2.resize(frame,(300,300)),1.0,(300,300),(104,177,123))
            face_net.setInput(blob)
            detections = face_net.forward()
            for i in range(detections.shape[2]):
                conf = detections[0,0,i,2]
                if conf > 0.5:
                    box = detections[0,0,i,3:7] * np.array([w_frame,h_frame,w_frame,h_frame])
                    fx1,fy1,fx2,fy2 = box.astype(int)
                    fx1,fy1 = max(0,fx1), max(0,fy1)
                    fx2,fy2 = min(w_frame,fx2), min(h_frame,fy2)
                    if fx2>fx1 and fy2>fy1:
                        face_roi = display[fy1:fy2, fx1:fx2]
                        display[fy1:fy2, fx1:fx2] = cv2.GaussianBlur(face_roi,(51,51),0)

        # ── Dashboard ──
        fps_counter += 1
        if time.time() - fps_start > 1.0:
            fps = fps_counter
            fps_counter = 0
            fps_start = time.time()

        cv2.line(display, (0,tripwire_y), (w_frame,tripwire_y), (0,0,255), 2)
        cv2.putText(display, f'FPS: {fps}', (10,30), cv2.FONT_HERSHEY_SIMPLEX,0.8,(0,255,255),2)
        cv2.putText(display, f'Objects: {len(objects)}', (10,60), cv2.FONT_HERSHEY_SIMPLEX,0.8,(0,255,255),2)
        cv2.putText(display, f'Crossings: {tripwire_count}', (10,90), cv2.FONT_HERSHEY_SIMPLEX,0.8,(0,255,255),2)
        if len(objects) > 0:
            cv2.putText(display, 'MOTION DETECTED', (10,120), cv2.FONT_HERSHEY_SIMPLEX,0.8,(0,0,255),2)

        cv2.imshow('Smart Surveillance System', display)
        cv2.imshow('Foreground Mask', fg_mask)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    vs.stop()
    cv2.destroyAllWindows()

if __name__ == '__main__':
    main()
```

---

## What This Proves

When you've built this, you have used in one working system:

- Color spaces and image properties (Part01-02)
- Geometric operations (Part03)
- Histogram and enhancement (Part04-05)
- Thresholding and filtering (Part06-07)
- Morphological operations (Part11)
- Contours (Part12)
- Background subtraction (Part21)
- Object tracking (Part22)
- Face detection (Part25)
- Video handling (Part20)
- Performance optimization (Part31)
- Debug visualization (Part30)

**That is fluency. That is the end of the curriculum. And the beginning of everything else.**
