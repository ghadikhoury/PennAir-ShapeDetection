# PennAir Shape Detection Challenge

This repository contains my implementation for the **PennAir Fall 2025 Software Challenge**.  
The goal was to build an algorithm that can detect, classify, and track solid shapes in both static images and dynamic videos on a noisy grassy background, and to extend the method to be background-agnostic.

---

## Project Overview

The pipeline was developed in **Python (OpenCV + NumPy)** and performs:

- **Detection** of geometric shapes (triangle, quadrilateral, pentagon, circle).  
- **Classification** using contour approximation.  
- **Tracking** with unique IDs and trajectories across video frames.  
- **Noise suppression** with blurring, morphology, and background subtraction.  
- **Background agnosticism** through adaptive thresholds and stronger contour filtering.

---

## 🔬 Development Process

1. **Initial Edge Detection (Canny, Sobel, Laplacian)**  
   - Tried classic edge detectors.  
   - Result: too sensitive to grass texture, produced splatters of false contours.

2. **Morphology & Filtering**  
   - Added Gaussian/Median blur + morphological open/close.  
   - Helped reduce noise but often removed shapes entirely if tuned too strictly.

3. **Switch to Background Subtraction (MOG2)**  
   - Used MOG2 with adaptive learning rates.  
   - Allowed the algorithm to model static grass and highlight moving shapes.  
   - Added hole-filling, contour filtering by area/solidity.

4. **Shape Classification**  
   - Approximated polygons with `cv2.approxPolyDP`.  
   - Classified by number of vertices, with AR check for squares.

5. **Tracking Layer**  
   - Assigned IDs to shapes.  
   - Matched detections frame-to-frame by type and centroid distance.  
   - Stored trajectories for visualization.

---

## Challenges & Fixes

- **Grass texture noise** → solved with median blur + morphology.  
- **Over-thresholding (shapes vanish)** → tuned percentiles + switched to Otsu in some tests.  
- **Phantom large contours** → introduced max area cutoff.  
- **Shapes dropping in/out** → tracking layer stabilized identity.  
- **Background agnosticism** → tuned solidity/area thresholds to remove clutter.


