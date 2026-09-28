# Part 3: Field-Optimized Features & Real-World Troubleshooting

> *"80% of software development is implementing features, but the final 20% of operational detail is what gives it enduring utility on the factory floor."*

## 1. Outlier Removal Filter & Interactive [Refresh]

Industrial X-ray and optical measurement feeds often suffer from pixel defects, lint, or particulate contamination, causing abnormal **spike noise**. A single wild pixel can distort the entire dynamic range of the 3D surface mesh.

![View 2D Outlier Filter](images/view2d_filter.png)

### Parallel 8-Neighbor Averaging (`RemoveOutliersAndFill`)
We implemented a multithreaded algorithm (`Parallel.For`) that identifies outlier pixels falling outside configured Low/High thresholds and smoothly replaces them with the mean value of their surrounding 8 valid neighbors.

### UX Refinement: The [Refresh] Button
While a simple toggle checkbox existed initially, field technicians requested: **"We want to tweak the threshold numbers incrementally and immediately preview the filtered topography in real-time."**
- Placing a dedicated **[Refresh] button** directly alongside the threshold inputs allows operators to trigger asynchronous (`Task.Run`) surface re-evaluations on demand.

> 🎬 **Outlier Filter in Action**: Check the filter tuning demo in our [🎬 Full Demo Video](en/demo-video.md) starting at **01:30**.

---

## 2. Real-Time Multi-ROI Step Arithmetic
Operators frequently need to answer: **"What is the net height difference between the datum floor (First) and the solder bump (Second)?"**

```
[ First: 24635.500 ]  ─  [ Second: 24540.885 ]  =  [ Result: 54.284 ]
```

* **One-Click Value Harvesting**: Clicking the sample button extracts the current ROI's mean height directly into the active operand field.
* **Instant Calculation**: Any change in operand values or arithmetic operators (`-`, `+`, `*`, `/`) immediately updates the result formatted to 3 decimal places.
* **Docking Stability**: The calculator strip is locked to a fixed 60px height at the bottom of the window, preventing layout jumps during window resizing.

---

## 3. Real-World Troubleshooting: Resolving `PlaneList` Index Out of Range Crash
When loading older project inspection files (`.x3d`), an unexpected `ArgumentOutOfRangeException` occurred during `PlaneFitting` calculation, causing the application to crash.

### Root Cause Analysis
In older file schemas or partially saved XML documents, `<CalcList>` recorded a `PlaneFitting` calculation entry, but the `<PlaneList />` container contained zero coordinate points.
The code blindly accessed array indices `[0], [1], [2]` without checking the list length.

### Resolution: Defensive Preconditions
```csharp
// Before: Blind index access without size check -> Crash risk!
XGlobal.FitPlaneFromThreePoints(workItem.PlaneList[0].X, ...);

// After: Comprehensive validation ensuring at least 3 points exist
if (workItem.PlaneList != null && workItem.PlaneList.Count >= 3)
{
    XGlobal.FitPlaneFromThreePoints(
        workItem.PlaneList[0].X, workItem.PlaneList[0].Y, workItem.PlaneList[0].Z,
        workItem.PlaneList[1].X, workItem.PlaneList[1].Y, workItem.PlaneList[1].Z,
        workItem.PlaneList[2].X, workItem.PlaneList[2].Y, workItem.PlaneList[2].Z,
        out double dA, out double dB, out double dC, out double dD);
    ...
}
else
{
    Console.WriteLine("PlaneFitting skipped: PlaneList does not contain at least 3 points.");
}
```

This defensive safeguard ensures legacy or malformed files are handled gracefully without application crashes.

---

## 4. Work Item Deletion & Canvas Synchronization
During complex comparative inspections, operators often generate temporary test ROIs that need removal:
- Support for both the **`Delete` key and right-click context menu** allows immediate deletion of selected work items.
- The root `Origin` image item is protected from deletion, and removing a work item immediately clears its 2D overlay boundary from the canvas for seamless visual consistency.
