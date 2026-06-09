# Mini Project — People Counter

## Goal
Fixed camera watches a doorway. Count people that cross a virtual line.

```python
import cv2
import numpy as np
from scipy.spatial.distance import cdist

class CentroidTracker:
    def __init__(self, max_gone=40, max_dist=80):
        self.next_id=0; self.objects={}; self.disappeared={}
        self.max_gone=max_gone; self.max_dist=max_dist
    def register(self,c):
        self.objects[self.next_id]=np.array(c); self.disappeared[self.next_id]=0; self.next_id+=1
    def deregister(self,oid):
        del self.objects[oid]; del self.disappeared[oid]
    def update(self, rects):
        if not rects:
            for oid in list(self.disappeared):
                self.disappeared[oid]+=1
                if self.disappeared[oid]>self.max_gone: self.deregister(oid)
            return self.objects
        new_c=np.array([(x+w//2,y+h//2) for x,y,w,h in rects])
        if not self.objects:
            for c in new_c: self.register(c)
            return self.objects
        ids=list(self.objects.keys()); old_c=np.array(list(self.objects.values()))
        D=cdist(old_c,new_c); rows=D.min(axis=1).argsort(); cols=D.argmin(axis=1)[rows]
        ur,uc=set(),set()
        for r,c in zip(rows,cols):
            if r in ur or c in uc or D[r,c]>self.max_dist: continue
            oid=ids[r]; self.objects[oid]=new_c[c]; self.disappeared[oid]=0; ur.add(r); uc.add(c)
        for r in set(range(len(ids)))-ur:
            self.disappeared[ids[r]]+=1
            if self.disappeared[ids[r]]>self.max_gone: self.deregister(ids[r])
        for c in set(range(len(new_c)))-uc: self.register(new_c[c])
        return self.objects

cap   = cv2.VideoCapture(0)
mog2  = cv2.createBackgroundSubtractorMOG2(history=400, varThreshold=50, detectShadows=False)
se    = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(5,5))
ct    = CentroidTracker()

ret, frame = cap.read()
h, w = frame.shape[:2]
LINE_Y = h // 2

prev_cents = {}
count_up = count_down = 0

while True:
    ret, frame = cap.read()
    if not ret: break

    fg = mog2.apply(frame)
    fg = cv2.morphologyEx(fg, cv2.MORPH_OPEN,  se)
    fg = cv2.morphologyEx(fg, cv2.MORPH_CLOSE, se)

    cnts, _ = cv2.findContours(fg, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    rects   = [cv2.boundingRect(c) for c in cnts if cv2.contourArea(c)>800]
    objects = ct.update(rects)

    for oid, cent in objects.items():
        cx, cy = int(cent[0]), int(cent[1])
        prev_cy = prev_cents.get(oid, cy)
        if prev_cy < LINE_Y <= cy:   count_down += 1
        elif prev_cy > LINE_Y >= cy: count_up   += 1
        prev_cents[oid] = cy
        cv2.circle(frame,(cx,cy),5,(0,255,0),-1)
        cv2.putText(frame,f'{oid}',(cx+6,cy-6),cv2.FONT_HERSHEY_SIMPLEX,0.5,(255,255,0),1)

    cv2.line(frame,(0,LINE_Y),(w,LINE_Y),(0,0,255),2)
    cv2.putText(frame,f'IN: {count_down}  OUT: {count_up}',(10,40),
                cv2.FONT_HERSHEY_SIMPLEX,1.2,(0,255,255),2)

    cv2.imshow('People Counter — Q to quit', frame)
    if cv2.waitKey(1)&0xFF==ord('q'): break

cap.release(); cv2.destroyAllWindows()
print(f"Final count — IN: {count_down}  OUT: {count_up}")
```
