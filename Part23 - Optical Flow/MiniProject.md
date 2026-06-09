# Mini Project — Motion Trail Painter

## Goal
Every moving object leaves a glowing colored trail showing its path history.

```python
import cv2
import numpy as np

cap = cv2.VideoCapture(0)
ret, old = cap.read()
old_gray = cv2.cvtColor(old, cv2.COLOR_BGR2GRAY)
p0 = cv2.goodFeaturesToTrack(old_gray, 200, 0.01, 10)
colors = np.random.randint(0,255,(200,3))
mask   = np.zeros_like(old)

lk = dict(winSize=(15,15),maxLevel=2,
          criteria=(cv2.TERM_CRITERIA_EPS|cv2.TERM_CRITERIA_COUNT,10,0.03))

while True:
    ret, frame = cap.read()
    if not ret or p0 is None: break
    gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
    p1, st, _ = cv2.calcOpticalFlowPyrLK(old_gray,gray,p0,None,**lk)
    if p1 is None: p0=cv2.goodFeaturesToTrack(gray,200,0.01,10); old_gray=gray; continue
    good_new=p1[st==1]; good_old=p0[st==1]
    for i,(n,o) in enumerate(zip(good_new,good_old)):
        a,b=n.ravel().astype(int); c,d=o.ravel().astype(int)
        mask=cv2.line(mask,(a,b),(c,d),colors[i%200].tolist(),2)
        frame=cv2.circle(frame,(a,b),3,colors[i%200].tolist(),-1)
    out=cv2.add(frame,mask)
    mask=(mask*0.95).astype(np.uint8)  # fade trails
    cv2.imshow('Motion Trails — Q to quit',out)
    old_gray=gray.copy(); p0=good_new.reshape(-1,1,2)
    if cv2.waitKey(1)&0xFF==ord('q'): break
cap.release(); cv2.destroyAllWindows()
```
