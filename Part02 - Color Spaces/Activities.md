# Part 02 — Activities: Color Spaces

---

## Activity 1 — Channel Split Gallery
Load any colorful image. Split into B, G, R channels. Display all three as grayscale images side by side. Then display the original.
**Question to answer:** Which channel has the most contrast for your specific image? Why?

---

## Activity 2 — The Four Spaces
Convert the same image to Grayscale, HSV, LAB, and YCrCb. Display all four plus original in a 1×5 grid.
```python
spaces = [gray, hsv, lab, ycrcb]
```
**Observe:** How does the same scene look completely different depending on how you describe it.

---

## Activity 3 — Isolate Red Objects
Find or take a photo with a red object (red apple, red shirt, red cup). Use HSV `inRange` to isolate only the red regions. Display the mask and the extracted object.
**Challenge:** Red wraps around in HSV (near 0 AND near 180). Handle both ranges.

---

## Activity 4 — Object Recoloring
Load an image with a clearly colored object. Change its color by:
1. Converting to HSV
2. Masking the object's hue range
3. Shifting the H value inside the mask
4. Converting back to BGR
Example: make a yellow banana look purple.

---

## Activity 5 — Brightness Without Color Change
Convert an image to LAB. Add 40 to the L channel only (clip at 255). Convert back. Compare to adding 40 to all BGR channels.
**What to observe:** LAB brightness boost preserves color saturation. BGR boost washes colors out.
