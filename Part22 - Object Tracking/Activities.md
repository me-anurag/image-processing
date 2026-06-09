# Part 22 — Activities: Object Tracking

## Activity 1 — Centroid Tracker on Video
Run your centroid tracker (from Lesson01) on a real video with moving objects.
Measure: how long does each track persist? At what point do IDs get switched or lost?

## Activity 2 — CSRT vs MOSSE Speed Test
Initialize both trackers on the same object. Run for 200 frames each. Measure average FPS.
MOSSE should be 5-10x faster. At what speed does CSRT tracking quality degrade?

## Activity 3 — People Counter
Set up a fixed camera. Draw a virtual line across the frame. Count people crossing it
using centroid tracking + tripwire logic. Test: walk past the camera 5 times. Does it count 5?

## Activity 4 — Disappear and Reappear
Track an object. Cover it with your hand for 2 seconds (60 frames). Uncover it.
Does the tracker recover the same ID? What is your `max_disappeared` value set to?

## Activity 5 — Multi-Object Initialization
Track 3 objects simultaneously with CSRT MultiTracker. Move them around, cross their paths.
Document exactly when and why IDs get confused.
