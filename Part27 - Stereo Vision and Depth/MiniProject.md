# Mini Project — Depth Portrait

## Goal
Use a stereo pair to compute disparity. Segment foreground vs background by depth.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def depth_portrait(left_path, right_path):
    left  = cv2.imread(left_path,  0)
    right = cv2.imread(right_path, 0)
    if left is None or right is None:
        print("Provide left.jpg and right.jpg stereo pair"); return

    stereo   = cv2.StereoSGBM_create(
        minDisparity=0, numDisparities=96, blockSize=11,
        P1=8*3*11**2, P2=32*3*11**2, disp12MaxDiff=1,
        uniquenessRatio=10, speckleWindowSize=100, speckleRange=32
    )
    disp     = stereo.compute(left,right).astype(np.float32)/16.0
    disp_norm= cv2.normalize(disp,None,0,255,cv2.NORM_MINMAX).astype(np.uint8)

    # Segment: pixels with disparity > median are foreground (closer)
    fg_mask = (disp > np.median(disp[disp>0])).astype(np.uint8)*255
    fg_mask = cv2.morphologyEx(fg_mask, cv2.MORPH_CLOSE,
                cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(15,15)))

    color_left = cv2.imread(left_path)
    result     = cv2.bitwise_and(color_left, color_left, mask=fg_mask)

    fig,axes=plt.subplots(1,3,figsize=(18,5))
    axes[0].imshow(cv2.cvtColor(color_left,cv2.COLOR_BGR2RGB)); axes[0].set_title('Left image'); axes[0].axis('off')
    axes[1].imshow(disp_norm,cmap='jet');                        axes[1].set_title('Disparity map'); axes[1].axis('off')
    axes[2].imshow(cv2.cvtColor(result,cv2.COLOR_BGR2RGB));      axes[2].set_title('Foreground by depth'); axes[2].axis('off')
    plt.show()

depth_portrait('left.jpg','right.jpg')
```
