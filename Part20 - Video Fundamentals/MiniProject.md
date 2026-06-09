# Mini Project — Time-Lapse Builder

## Goal
Read a long video. Sample 1 frame every N frames. Reassemble into a short time-lapse. Add timestamp.

```python
import cv2
import numpy as np
from datetime import timedelta

def make_timelapse(input_path, output_path='timelapse.mp4', sample_every=30, output_fps=30):
    cap    = cv2.VideoCapture(input_path)
    fps    = cap.get(cv2.CAP_PROP_FPS)
    width  = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
    total  = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))

    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    out    = cv2.VideoWriter(output_path, fourcc, output_fps, (width, height))

    frame_idx = 0
    saved     = 0
    while True:
        ret, frame = cap.read()
        if not ret: break

        if frame_idx % sample_every == 0:
            orig_time = timedelta(seconds=frame_idx/fps)
            ts        = str(orig_time).split('.')[0]
            cv2.putText(frame, ts, (10, height-20),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.8, (255,255,255), 2)
            out.write(frame)
            saved += 1

        frame_idx += 1
        if frame_idx % 1000 == 0:
            print(f"  Progress: {frame_idx}/{total} frames")

    cap.release(); out.release()
    print(f"Time-lapse saved: {saved} frames → {saved/output_fps:.1f}s at {output_fps}fps")
    print(f"Compression ratio: {total}/{saved} = {total//saved}x speed-up")

make_timelapse('long_video.mp4', sample_every=60)
```
