# 🎬 XMat3D Analyzer 전체 동작 시연 영상

2D 단면 영상(TIF) 로딩부터 관심 영역(ROI) 지정, OpenTK 기반 실시간 3D 표면 메쉬 시각화, 3점 평면 피팅(Plane Fitting) 및 단차 계측까지의 전체 동작 시연입니다.

<div align="center">
  <video width="100%" controls preload="metadata" style="max-height: 520px; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.15);">
    <source src="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" type="video/mp4">
    <source src="videos/XMat3D_demo.mp4" type="video/mp4">
    브라우저가 HTML5 비디오 재생을 지원하지 않습니다.
  </video>
  <p><em>▲ XMat3D Analyzer 시연 영상 (재생시간: 3분 44초)</em></p>
  <p><a href="https://choidaeyong1231.github.io/XMat3D-blog/videos/XMat3D_demo.mp4" target="_blank">🔗 새 창에서 원본 영상 전체화면으로 보기 (다운로드)</a></p>
</div>

---

## ⏱️ 주요 기능 타임라인

| 시간 | 기능 | 상세 내용 |
| :--- | :--- | :--- |
| **00:00 ~ 00:45** | **2D 영상 로딩 & ROI 지정** | 16-bit Grayscale 단면 영상을 로드하고 원하는 검사 영역을 마우스로 드래그하여 지정 |
| **00:45 ~ 01:30** | **실시간 3D 메쉬 생성 & 뷰 제어** | OpenTK(OpenGL VBO)를 통해 생성된 3D 표면을 마우스로 360도 회전/확대/이동 |
| **01:30 ~ 02:20** | **이상치(Outlier) 제거 필터** | 지정 임계값을 벗어나는 스파이크 노이즈를 8방향 이웃 평균값으로 보간 처리 |
| **02:20 ~ 03:00** | **3점 평면 피팅(Plane Fitting)** | 3개의 기준점을 찍어 기준 평면을 수립하고, 기울어진 시편의 수평 단차를 정밀 계측 |
| **03:00 ~ 03:44** | **사칙연산 & 작업 이력 관리** | 다중 ROI 간의 평균 높이 뺄셈(단차) 즉시 연산 및 Work Item History 관리 |

---

> 📖 각 기능의 상세한 알고리즘과 구현 코드는 왼쪽 메뉴의 **실전 개발기 1편, 2편, 3편**에서 확인하실 수 있습니다.
