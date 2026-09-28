# XMat3D Analyzer

> **Real-time 3D height metrology and surface topography analysis system based on 2D X-Ray and high-resolution cross-sectional images.**

<p>
  <a href="https://choidaeyong1231.github.io/XMat3D-blog/#/en/"><img src="https://komarev.com/ghpvc/?username=choidaeyong1231-xmat3d&label=Visitors&color=007acc" alt="Visitors" /></a>
  <a href="https://github.com/choidaeyong1231/XMat3D-blog/issues/new"><img src="https://img.shields.io/badge/Q%26A-GitHub_Issues-brightgreen?logo=github&logoColor=white" alt="Ask Question" /></a>
</p>

<div style="margin: 16px 0;">
  <a href="https://raw.githubusercontent.com/choidaeyong1231/XMat3D-blog/main/downloads/XMat3DAnalyzer_Setup_v1.0.0.exe" download style="display: inline-block; padding: 11px 22px; background-color: #2ea44f; color: white; text-decoration: none; border-radius: 6px; font-weight: bold; font-size: 15px; box-shadow: 0 4px 12px rgba(46,164,79,0.35);">
    📥 Download Official XMat3D Analyzer v1.0.0 Setup (.exe, 22.9MB)
  </a>
</div>

![XMat3D Analyzer Main Overview](images/main_overview.png)

## 🎬 Live Demonstration Video

<div align="center">
  <video width="100%" controls preload="metadata" style="max-height: 480px; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.15);">
    <source src="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" type="video/mp4">
    <source src="videos/XMat3D_demo.mp4" type="video/mp4">
    Your browser does not support HTML5 video playback.
  </video>
  <p><em>▲ Real-time 3D mesh generation, ROI analysis, and step height metrology (3m 44s)</em></p>
  <p><small><a href="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" target="_blank">🔗 Watch raw video in new tab</a></small></p>
</div>

## 📌 Project Overview
**XMat3D Analyzer** is an industrial metrology and analysis application deployed in semiconductor fabrication, advanced electronic packaging, battery manufacturing, and precision optical inspection lines.

Without requiring capital-intensive 3D CT reconstruction machines or cumbersome commercial packages, XMat3D Analyzer **instantly converts any region of interest (ROI) from 2D inspection images (such as 16-bit Grayscale TIFFs) into an interactive 60 FPS 3D surface mesh**, enabling precise step height calculation, 3-point tilt compensation, and outlier noise filtration.

---

## ✨ Key Features

| Feature | Description |
| :--- | :--- |
| **Real-Time 3D Mesh Rendering** | High-throughput 60 FPS 3D surface visualization of selected 2D ROIs using OpenTK (OpenGL VBO) |
| **3-Point Plane Fitting** | Robust plane regression ($Ax + By + Cz + D = 0$) across three reference datums to compensate specimen tilt |
| **Outlier Filtration** | Parallel 8-neighbor averaging routine replacing spike noise outside Low/High thresholds |
| **Real-Time Height Arithmetic** | Instant arithmetic calculation (addition, difference/step height, multiplication, division) between multiple ROIs |
| **Multi-Format Data Export** | Exports ROI-annotated TIF, 3D viewport capture TIF, and 32-bit heightmap raw TIFF files |
| **Work Item History** | Lossless persistence and restoration of inspection parameters and ROI configurations (`.x3d`) |

---

## 🛠️ Tech Stack
* **Language & Framework**: C# 7.3, .NET Framework 4.7.2, Windows Forms
* **Computer Vision**: OpenCvSharp 3.x
* **3D Graphics Engine**: OpenTK (OpenGL Vertex Buffer Object / Shader-based GPU rendering)
* **UI Framework**: Krypton Toolkit (Visual Studio-style docking window system)

---

## 📖 Engineering Tech Blog Series
A deep dive into our architectural decisions, graphics optimization techniques, and real-world shop-floor troubleshooting:

1. **[Part 1: Why We Built a 3D Inspection Tool from 2D Images](/en/01-why-we-build.md)**
   - The motivation behind overcoming 2D visual limitations and our core design goals.
2. **[Part 2: OpenTK High-Speed Mesh Rendering & Step Analysis](/en/02-opentk-mesh-rendering.md)**
   - Converting 16-bit TIFF datasets into GPU vertex buffers and fitting planar datums.
3. **[Part 3: Field-Optimized Features & Real-World Troubleshooting](/en/03-features-and-troubleshooting.md)**
   - The critical final 20% of operational details: outlier filtering and defensive crash prevention.

---

## 📬 Contact & Support

For project collaboration inquiries, algorithm integration, or technical questions:

👉 **[📝 Open a Question / Discussion on GitHub Issues](https://github.com/choidaeyong1231/XMat3D-blog/issues/new)**  
*(We manage inquiries through GitHub Issues to prevent spam and maintain transparent communication)*
