# 2편: OpenTK 고속 메쉬 렌더링과 단차 분석

> *"수십만 개의 3D 포인트를 C# WinForms 위에서 부드럽게 돌리려면 어떻게 해야 할까?"*

## 1. 2D 픽셀을 3D 정점(Vertex)으로 변환하기
2D 영상의 픽셀 좌표 $(X, Y)$와 픽셀 밝기값(Intensity, $Z$)을 3차원 공간의 정점 $(X, Y, Z)$로 사상(Mapping)합니다.

이때 중요한 것은 **Z축 깊이 스케일링(Depth Scale)**과 **샘플링(Sampling Size)** 최적화입니다:
- 모든 픽셀을 1:1로 3D 정점으로 만들면 정점 개수가 수백만 개를 넘어 GPU 렌더링에 부담이 됩니다.
- 이미지 크기에 따라 적응형(Auto Sampling) 간격을 계산하여 그리드 메쉬(Grid Mesh)를 생성합니다.

```csharp
// 그리드 상의 정점(Vertex) 및 노멀(Normal) 벡터 산출
for (int y = 0; y < nRows; y += nSampling)
{
    for (int x = 0; x < nCols; x += nSampling)
    {
        float fZ = (float)matData.At<ushort>(y, x) * fDepthScale;
        vertices.Add(new Vector3(x, y, fZ));
    }
}
```

---

## 2. OpenTK VBO(Vertex Buffer Object)를 통한 고속 렌더링
C# WinForms 환경에서 전통적인 `glBegin() / glEnd()` 방식(Immediate Mode)을 사용하면 프레임워크 간 전환 오버헤드로 인해 프레임 레이트가 심각하게 떨어집니다.

이를 해결하기 위해 **OpenTK의 VBO(Vertex Buffer Object)**를 적용했습니다:
1. 계산된 모든 정점 좌표, 컬러, 법선(Normal) 벡터를 GPU 메모리에 단 한 번 전송.
2. 마우스로 회전/줌할 때는 CPU 연산 없이 GPU 내부 버퍼만 참조해 초당 60프레임 이상으로 부드럽게 렌더링.
3. 조명(Phong Shading)과 높이별 레인보우 컬러맵을 입혀 표면 단차를 극대화.

---

## 3. 3점 기반 평면 피팅 (Plane Fitting)
부품이 측정 테이블 위에서 미세하게 기울어져 있으면, 순수한 높이 계측에 오차가 생깁니다.

이를 보정하기 위해 사용자가 기준면 위의 **3개 점($P_1, P_2, P_3$)**을 클릭하면, 3차원 평면 방정식($Ax + By + Cz + D = 0$)을 풀어서 전체 표면의 기울기를 평탄화(Flattening)하는 기능을 구현했습니다.

```csharp
// 세 점을 지나는 평면의 법선 벡터(A, B, C)와 D 계산
public static void FitPlaneFromThreePoints(
    double x1, double y1, double z1,
    double x2, double y2, double z2,
    double x3, double y3, double z3,
    out double dA, out double dB, out double dC, out double dD)
{
    // 두 벡터 V1, V2 생성 후 외적(Cross Product)
    double v1x = x2 - x1, v1y = y2 - y1, v1z = z2 - z1;
    double v2x = x3 - x1, v2y = y3 - y1, v2z = z3 - z1;

    dA = v1y * v2z - v1z * v2y;
    dB = v1z * v2x - v1x * v2z;
    dC = v1x * v2y - v1y * v2x;
    dD = -(dA * x1 + dB * y1 + dC * z1);
}
```

산출된 평면 방정식을 바탕으로 각 픽셀과 평면 사이의 수직 거리를 계산하면, **기울어진 부품의 경사가 완벽히 보정된 순수 높이 프로파일**을 얻을 수 있습니다.
