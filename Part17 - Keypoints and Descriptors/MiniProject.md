# Mini Project — Visual Fingerprint

## Goal
Extract keypoints and descriptors from an object photo. Save them.
Photograph the same object again from a different angle. Load saved descriptors.
Prove they match across viewpoints.

```python
import cv2
import numpy as np
import pickle
import matplotlib.pyplot as plt

def create_fingerprint(image_path, save_path='fingerprint.pkl'):
    img  = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    orb  = cv2.ORB_create(nfeatures=1000)
    kps, descs = orb.detectAndCompute(gray, None)
    data = {'keypoints': [(kp.pt, kp.size, kp.angle, kp.response) for kp in kps],
            'descriptors': descs, 'image': img}
    with open(save_path, 'wb') as f:
        pickle.dump(data, f)
    print(f"Fingerprint saved: {len(kps)} keypoints → {save_path}")
    return kps, descs, img

def match_fingerprint(query_path, fingerprint_path='fingerprint.pkl'):
    with open(fingerprint_path, 'rb') as f:
        data = pickle.load(f)

    query = cv2.imread(query_path)
    gray  = cv2.cvtColor(query, cv2.COLOR_BGR2GRAY)
    orb   = cv2.ORB_create(nfeatures=1000)
    kps_q, descs_q = orb.detectAndCompute(gray, None)

    stored_kps = [cv2.KeyPoint(x=pt[0][0], y=pt[0][1], size=pt[1], angle=pt[2], response=pt[3])
                  for pt in data['keypoints']]

    bf      = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=False)
    matches = bf.knnMatch(data['descriptors'], descs_q, k=2)
    good    = [m for m,n in matches if m.distance < 0.75*n.distance]

    vis = cv2.drawMatches(data['image'], stored_kps, query, kps_q,
                          good[:30], None, flags=2)

    plt.figure(figsize=(18,6))
    plt.imshow(cv2.cvtColor(vis, cv2.COLOR_BGR2RGB))
    plt.title(f'Good matches: {len(good)} / {len(matches)} — {"MATCH" if len(good)>10 else "NO MATCH"}')
    plt.axis('off'); plt.show()
    return len(good)

create_fingerprint('object_view1.jpg')
n = match_fingerprint('object_view2.jpg')
print(f"Result: {n} good matches")
```
