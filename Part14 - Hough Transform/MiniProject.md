# Part14 - Hough Transform — Mini Project

**Why this part:** Detect perfect geometric shapes even when they're broken, noisy, or incomplete.

## Project Goal
Build a complete, working application using the techniques from all lessons in this part.

## Requirements
- Uses all major concepts from this part's lessons
- Runs on a real image or webcam input
- Produces a meaningful, visible output
- Includes before/after comparison where applicable

## Starter Structure
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def main():
    # Load input
    img = cv2.imread('your_input.jpg')

    # ── Your pipeline here ──

    # Show result
    cv2.imwrite('output.jpg', img)
    print("Done")

if __name__ == '__main__':
    main()
```

## Extension Challenges
1. Add a real-time webcam version
2. Process a full folder of images
3. Add parameter tuning via trackbars
4. Combine with techniques from previous parts
