# Mini Project — The Impossible Photo

## Goal
Seamlessly blend an object from one photo into another so it looks real.

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def impossible_photo(background_path, object_path, mask_path=None):
    bg  = cv2.imread(background_path)
    obj = cv2.imread(object_path)
    if bg is None or obj is None: print("Cannot load images"); return

    # Resize object to fit in background if needed
    h_bg,w_bg = bg.shape[:2]
    h_ob,w_ob = obj.shape[:2]
    scale     = min(h_bg*0.4/h_ob, w_bg*0.4/w_ob)
    obj       = cv2.resize(obj, (int(w_ob*scale),int(h_ob*scale)))
    h_ob,w_ob = obj.shape[:2]

    # Create or load mask
    if mask_path:
        mask = cv2.imread(mask_path,0)
        mask = cv2.resize(mask,(w_ob,h_ob))
    else:
        mask = np.zeros((h_ob,w_ob),dtype=np.uint8)
        cv2.rectangle(mask,(5,5),(w_ob-5,h_ob-5),255,-1)  # simple rectangle mask

    # Center of object in background
    cx,cy = w_bg//2, h_bg//2
    center = (cx,cy)

    # Seamless clone
    result = cv2.seamlessClone(obj, bg, mask, center, cv2.NORMAL_CLONE)

    fig,axes=plt.subplots(1,3,figsize=(18,6))
    axes[0].imshow(cv2.cvtColor(bg, cv2.COLOR_BGR2RGB));     axes[0].set_title('Background'); axes[0].axis('off')
    axes[1].imshow(cv2.cvtColor(obj,cv2.COLOR_BGR2RGB));     axes[1].set_title('Object');     axes[1].axis('off')
    axes[2].imshow(cv2.cvtColor(result,cv2.COLOR_BGR2RGB));  axes[2].set_title('Seamlessly Blended!'); axes[2].axis('off')
    plt.suptitle('Poisson seamless cloning — lighting adapts automatically',fontsize=12)
    plt.tight_layout(); plt.show()
    cv2.imwrite('impossible_photo.jpg',result)
    print("Saved: impossible_photo.jpg")

impossible_photo('background.jpg','object.jpg')
```
