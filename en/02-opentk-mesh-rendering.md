# Part 2: OpenTK High-Speed Mesh Rendering & Step Analysis

> *"How can we smoothly orbit hundreds of thousands of 3D points inside a C# WinForms environment?"*

## 1. Transforming 2D Pixels into 3D Vertices
Every 2D pixel coordinate $(X, Y)$ and its intensity value ($Z$) are mapped into a 3D coordinate space $(X, Y, Z)$.

Crucial to this transformation are **Depth Scaling** and **Adaptive Sampling**:
- Converting every individual pixel 1:1 into 3D vertices creates millions of points, which overloads the GPU bus and reduces frame rates.
- We calculate an adaptive sampling step based on ROI dimensions to construct an optimized grid mesh.

```csharp
// Generating grid vertices and normal vectors
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

## 2. High-Speed OpenTK VBO (Vertex Buffer Object) Rendering
Using legacy OpenGL immediate mode (`glBegin() / glEnd()`) in C# WinForms results in severe frame drops due to managed/unmanaged interop overhead per frame.

To achieve silky interaction, we adopted **OpenTK's VBO (Vertex Buffer Object)** architecture:
1. All vertex positions, colors, and surface normals are transferred to GPU VRAM in a single upload.
2. During interactive camera orbiting and zooming, the GPU renders purely from its internal buffer without incurring CPU computation, sustaining 60+ FPS.
3. Specular Phong shading combined with height-based rainbow palettes amplifies subtle surface topological variations.

> 🎬 **Interactive Viewport Demo**: Notice the fluid 3D mesh interaction in our [🎬 Full Demo Video](en/demo-video.md) starting at **00:45**.

---

## 3. 3-Point Planar Tilt Fitting (Plane Fitting)
When a specimen is slightly angled on the sample stage, pure vertical measurements accumulate tilt errors.

To compensate for stage misalignments, operators can click **3 reference points ($P_1, P_2, P_3$)** on a datum plane. The system solves the 3D plane equation ($Ax + By + Cz + D = 0$) to flatten the surface mathematically.

```csharp
// Calculating plane normal vector (A, B, C) and offset D through 3 points
public static void FitPlaneFromThreePoints(
    double x1, double y1, double z1,
    double x2, double y2, double z2,
    double x3, double y3, double z3,
    out double dA, out double dB, out double dC, out double dD)
{
    // Generate vectors V1, V2 and evaluate cross product
    double v1x = x2 - x1, v1y = y2 - y1, v1z = z2 - z1;
    double v2x = x3 - x1, v2y = y3 - y1, v2z = z3 - z1;

    dA = v1y * v2z - v1z * v2y;
    dB = v1z * v2x - v1x * v2z;
    dC = v1x * v2y - v1y * v2x;
    dD = -(dA * x1 + dB * y1 + dC * z1);
}
```

Evaluating the perpendicular distance between each surface point and the fitted plane yields a **true height profile entirely isolated from physical sample tilt**.
