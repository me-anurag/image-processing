# CAPSTONE — Smart Surveillance System

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 860 200" width="860" height="200" font-family="'Courier New', Courier, monospace">
  <rect width="860" height="200" fill="#0a0a0a"/>
  <g stroke="#1a1a1a" stroke-width="1">
    <line x1="0" y1="40" x2="860" y2="40"/>
    <line x1="0" y1="80" x2="860" y2="80"/>
    <line x1="0" y1="120" x2="860" y2="120"/>
    <line x1="0" y1="160" x2="860" y2="160"/>
  </g>
  <line x1="0" y1="100" x2="860" y2="100" stroke="#00E5FF" stroke-width="0.5" stroke-dasharray="4 6" opacity="0.25"/>
  <defs>
    <marker id="arrow" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
      <path d="M0,0 L0,6 L6,3 Z" fill="#00E5FF" opacity="0.7"/>
    </marker>
  </defs>
  <g stroke="#00E5FF" stroke-width="1" opacity="0.5" marker-end="url(#arrow)">
    <line x1="108" y1="100" x2="123" y2="100"/>
    <line x1="203" y1="100" x2="218" y2="100"/>
    <line x1="298" y1="100" x2="313" y2="100"/>
    <line x1="393" y1="100" x2="408" y2="100"/>
    <line x1="488" y1="100" x2="503" y2="100"/>
    <line x1="583" y1="100" x2="598" y2="100"/>
    <line x1="678" y1="100" x2="693" y2="100"/>
  </g>
  <rect x="8" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="8" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="58" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">MOG2</text>
  <text x="58" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Motion</text>
  <rect x="128" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="128" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="178" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">CONTOURS</text>
  <text x="178" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Segment</text>
  <rect x="248" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="248" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="298" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">CENTROID</text>
  <text x="298" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Track</text>
  <rect x="368" y="74" width="100" height="52" rx="2" fill="#111" stroke="#00E5FF" stroke-width="1" opacity="0.9"/>
  <rect x="368" y="74" width="100" height="3" rx="1" fill="#00E5FF"/>
  <text x="418" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">LINE CROSS</text>
  <text x="418" y="112" text-anchor="middle" fill="#00E5FF" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Tripwire</text>
  <rect x="488" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="488" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="538" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">DNN SSD</text>
  <text x="538" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Face → Blur</text>
  <rect x="608" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="608" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="658" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">DISK LOG</text>
  <text x="658" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Snapshot</text>
  <rect x="728" y="74" width="100" height="52" rx="2" fill="#111" stroke="#333" stroke-width="1"/>
  <rect x="728" y="74" width="100" height="3" rx="1" fill="#00E5FF" opacity="0.8"/>
  <text x="778" y="97" text-anchor="middle" fill="#888" font-size="8" letter-spacing="1" font-family="'Courier New', Courier, monospace">OVERLAY</text>
  <text x="778" y="112" text-anchor="middle" fill="#eee" font-size="9" font-weight="bold" font-family="'Courier New', Courier, monospace">Dashboard</text>
  <text x="12" y="170" fill="#444" font-size="8" letter-spacing="2" font-family="'Courier New', Courier, monospace">WEBCAM INPUT</text>
  <text x="848" y="170" fill="#444" font-size="8" letter-spacing="2" text-anchor="end" font-family="'Courier New', Courier, monospace">LIVE OUTPUT</text>
  <g fill="#333" font-size="7" letter-spacing="0.5" font-family="'Courier New', Courier, monospace">
    <text x="12" y="87">01</text>
    <text x="132" y="87">02</text>
    <text x="252" y="87">03</text>
    <text x="372" y="87">04</text>
    <text x="492" y="87">05</text>
    <text x="612" y="87">06</text>
    <text x="732" y="87">07</text>
  </g>
</svg>

A real-time webcam application running the complete classical computer vision pipeline — motion detection, object tracking, face anonymization, tripwire counting, and live dashboard overlay. Built as the capstone of a 32-part image processing curriculum.

---

## Features

| Feature | Technique |
|---|---|
| Motion detection | Background subtraction (MOG2) |
| Object segmentation | Contour detection on foreground mask |
| Object tracking | Centroid tracker with persistent IDs |
| Face detection | DNN face detector (SSD ResNet-10) |
| Face anonymization | Gaussian blur on face ROI |
| Tripwire counting | Line crossing detection |
| Snapshot logging | Frame saving with timestamp |
| Live dashboard | Text overlay, FPS, object count |
| Real-time performance | Threaded capture with frame queue |

---

## Requirements

```
opencv-python
numpy
```

The face detector requires two model files downloaded separately:

```
deploy.prototxt
res10_300x300_ssd_iter_140000.caffemodel
```

Download from the [OpenCV samples directory](https://github.com/opencv/opencv/tree/master/samples/dnn/face_detector). Place both files in the project root. The system runs without them — face detection is disabled gracefully if the files are missing.

---

## Usage

```bash
pip install opencv-python numpy
python main.py
```

Press `q` to quit. Snapshots of new objects are saved to `snapshots/` automatically.

---

## Configuration

At the top of `main.py`:

```python
TRIPWIRE_Y = 0.5        # tripwire position as fraction of frame height
MIN_CONTOUR_AREA = 800  # minimum pixel area to track an object
```

---

## Build Order

Each step depends on the previous. Do not skip.

**Step 1 — Motion Detection**
Get MOG2 running. Verify a clean foreground mask. Add morphological cleanup (opening + dilation).

**Step 2 — Contour Detection**
Find contours in the foreground mask. Filter by minimum area. Draw bounding boxes.

**Step 3 — Centroid Tracker**
Assign each contour a persistent ID using centroid distance matching. IDs survive small movements.

**Step 4 — Tripwire**
Draw a horizontal line across the frame. Count when a tracked centroid crosses it.

**Step 5 — Face Detection**
For each tracked object's bounding box ROI, run the DNN face detector. Blur any face found.

**Step 6 — Snapshot Logging**
When a new object ID appears, save a timestamped snapshot to disk.

**Step 7 — Dashboard Overlay**
Display FPS, active object count, total crossings, and alert status on the live frame.

**Step 8 — Threading**
Move `VideoCapture` to a separate thread feeding a queue. Measure FPS improvement.

---

## Implementation

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

## Curriculum Coverage

This capstone exercises every major topic from the 32-part curriculum in a single working system.

| Parts | Topic |
|---|---|
| 01–02 | Color spaces and image properties |
| 03 | Geometric operations |
| 04–05 | Histogram and enhancement |
| 06–07 | Thresholding and filtering |
| 11 | Morphological operations |
| 12 | Contours |
| 20 | Video handling and frame I/O |
| 21 | Background subtraction |
| 22 | Object tracking |
| 25 | Face detection |
| 30 | Debug visualization |
| 31 | Performance optimization |

---

> That is fluency. That is the end of the curriculum. And the beginning of everything else.
