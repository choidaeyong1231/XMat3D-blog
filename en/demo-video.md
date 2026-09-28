# 🎬 XMat3D Analyzer Live Demonstration Video

A complete walkthrough video showing 2D cross-sectional image (TIF) loading, Region of Interest (ROI) selection, OpenTK-powered real-time 3D surface mesh visualization, 3-point plane fitting, and step height metrology.

<div align="center">
  <video width="100%" controls preload="metadata" style="max-height: 520px; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.15);">
    <source src="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" type="video/mp4">
    <source src="videos/XMat3D_demo.mp4" type="video/mp4">
    Your browser does not support HTML5 video playback.
  </video>
  <p><em>▲ XMat3D Analyzer Demo Video (Duration: 3m 44s)</em></p>
  <p><a href="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" target="_blank">🔗 Watch / Download raw video file in full screen</a></p>
</div>

---

## ⏱️ Feature Timeline & Highlights

| Timestamp | Feature | Technical Details |
| :--- | :--- | :--- |
| **00:00 ~ 00:45** | **2D Image Loading & ROI Selection** | Loads 16-bit Grayscale cross-sectional image and interactively defines rectangular inspection areas via mouse dragging |
| **00:45 ~ 01:30** | **Real-Time 3D Mesh & Viewport Control** | Generates 3D surface meshes using OpenTK (OpenGL VBO) with 360° orbit, pan, and zoom at 60 FPS |
| **01:30 ~ 02:20** | **Outlier Removal Filter** | Identifies spike noise exceeding threshold limits and performs parallel 8-neighbor averaging interpolation |
| **02:20 ~ 03:00** | **3-Point Plane Fitting** | Samples 3 planar datum points to establish a reference plane, mathematically compensating stage tilt |
| **03:00 ~ 03:44** | **Step Height Arithmetic & Work History** | Instant subtraction arithmetic between multiple ROIs and history management for reproducible inspection |

---

> 📖 Detailed algorithms and implementation code are documented across **Parts 1, 2, and 3** in the sidebar.
