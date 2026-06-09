# Mini Project — Book Finder

## Goal
Photograph 5 book covers. Build a recognition system. Query with a new photo. System identifies which book.

```python
import cv2
import numpy as np
import os
import matplotlib.pyplot as plt

class BookFinder:
    def __init__(self):
        self.orb   = cv2.ORB_create(nfeatures=1000)
        self.bf    = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
        self.books = {}  # name -> (keypoints, descriptors, image)

    def register(self, name, image_path):
        img  = cv2.imread(image_path)
        gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        kps, descs = self.orb.detectAndCompute(gray, None)
        if descs is not None:
            self.books[name] = (kps, descs, img)
            print(f"Registered '{name}': {len(kps)} keypoints")

    def identify(self, query_path, top_k=3):
        query = cv2.imread(query_path)
        gray  = cv2.cvtColor(query, cv2.COLOR_BGR2GRAY)
        kps_q, descs_q = self.orb.detectAndCompute(gray, None)
        if descs_q is None: return

        scores = {}
        for name, (kps_r, descs_r, img_r) in self.books.items():
            matches = self.bf.match(descs_r, descs_q)
            good    = [m for m in matches if m.distance < 60]
            scores[name] = len(good)

        ranked = sorted(scores.items(), key=lambda x: x[1], reverse=True)
        print("\nIdentification results:")
        for name, score in ranked:
            print(f"  {name}: {score} matches")

        best_name = ranked[0][0]
        best_img  = self.books[best_name][2]
        vis = cv2.drawMatches(best_img, self.books[best_name][0], query, kps_q,
            self.bf.match(self.books[best_name][1], descs_q)[:30], None, flags=2)
        plt.figure(figsize=(16,6))
        plt.imshow(cv2.cvtColor(vis, cv2.COLOR_BGR2RGB))
        plt.title(f'Best match: {best_name} ({ranked[0][1]} good matches)'); plt.axis('off'); plt.show()

finder = BookFinder()
# Register your books:
# finder.register('Python Crash Course', 'book1.jpg')
# finder.register('Clean Code', 'book2.jpg')
# Then query:
# finder.identify('query_book.jpg')
print("Register books with finder.register('name', 'path') then call finder.identify('query.jpg')")
