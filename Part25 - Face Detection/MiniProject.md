# Mini Project — Attendance Blur

## Goal
Take any video. Detect all faces every frame using DNN. Blur each face. Save anonymized video.

```python
import cv2
import numpy as np
import os

def anonymize_video(input_path, output_path='anonymized.mp4', conf_thresh=0.5):
    if not os.path.exists('deploy.prototxt'):
        print("Download face model files first (see Lesson04)")
        return

    net   = cv2.dnn.readNetFromCaffe('deploy.prototxt','face_model.caffemodel')
    cap   = cv2.VideoCapture(input_path)
    fps   = cap.get(cv2.CAP_PROP_FPS)
    w,h   = int(cap.get(3)),int(cap.get(4))
    out   = cv2.VideoWriter(output_path, cv2.VideoWriter_fourcc(*'mp4v'), fps, (w,h))

    while True:
        ret, frame = cap.read()
        if not ret: break
        blob = cv2.dnn.blobFromImage(cv2.resize(frame,(300,300)),1.0,(300,300),(104,177,123))
        net.setInput(blob); dets=net.forward()
        for i in range(dets.shape[2]):
            if dets[0,0,i,2]>conf_thresh:
                box=(dets[0,0,i,3:7]*np.array([w,h,w,h])).astype(int)
                x1,y1,x2,y2=np.clip(box,[0,0,0,0],[w,h,w,h])
                if x2>x1 and y2>y1:
                    frame[y1:y2,x1:x2]=cv2.GaussianBlur(frame[y1:y2,x1:x2],(51,51),0)
        out.write(frame)

    cap.release(); out.release()
    print(f"Saved: {output_path}")

anonymize_video('meeting_recording.mp4')
```
