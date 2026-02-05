# Angle Printing Summary (角度打印总结)

This document describes the changes made to print the angles mentioned in the paper.

## Changes Made

### 1. Sampling Angles (采样角度) - box_proposal_detail.cpp

**Location:** Line 152-163

Added print statements to show:
- **Sampling angle step** (采样角度步长): 6.0 degrees
- **Number of samples** (采样数量): Size of obj_yaw_samples vector
- **Individual sampling angles** (采样角度列表): Each angle value in degrees

These correspond to the sampling angles mentioned in the paper that vary between 1° to 4° (the paper mentions variable step sizes, while the code uses a fixed 6° step).

**Output format:**
```
===== Object Yaw Sampling Angles (物体朝向采样角度) =====
Sampling angle step (采样角度步长): 6 degrees
Number of samples (采样数量): [number]
Sampling angles (采样角度列表, in degrees):
  Sample 0: [angle] degrees
  Sample 1: [angle] degrees
  ...
=================================================
```

### 2. Angle Differences Θ_tl and Θ_tw - object_3d_util.cpp

**Location:** Function `box_edge_alignment_angle_error`, lines 527-586

Added print statements to show:
- **Individual angle differences** for each VP (vanishing point) and edge
- These correspond to Θ_tl and Θ_tw mentioned in equation (10) of the paper
- The code calculates the minimum angle difference between box edges and line segments

**Output format:**
```
===== Angle Error Calculation (角度误差计算) =====
VP 0 Edge 0 angle diff (角度差 Θ): [value] degrees
VP 0 Edge 1 angle diff (角度差 Θ): [value] degrees
...
Total angle error (总角度误差 Θ_e): [value] degrees
=================================================
```

### 3. Final Edge Angle Error - box_proposal_detail.cpp

**Location:** Line 571-579

Added print statements to show:
- **edge_angle_error** for sample objects
- This is the final angle error stored in each cuboid object
- Limited to first 10 samples to avoid excessive output

**Output format:**
```
Sample object 0 edge_angle_error (边缘角度误差): [value] degrees
Sample object 1 edge_angle_error (边缘角度误差): [value] degrees
...
```

## Equation (10) Implementation

The paper defines the angle error as:

```
Θ_e = (Σ_{t=1}^{N_p} min(Θ_tl, Θ_tw)) / N_p
```

Where:
- min(⋅) takes the smaller of two parameters
- Θ_tl and Θ_tw are angle differences between the t-th line segment and the projected cube's long/wide edges
- N_p is the number of line segments

This is implemented in the `box_edge_alignment_angle_error` function, which:
1. Iterates over vanishing points (VPs)
2. For each VP, calculates angle differences for edges
3. Takes the minimum angle difference using `std::min(temp, M_PI-temp)`
4. Accumulates the total angle difference

## Files Modified

1. **src/detect_3d_cuboid/box_proposal_detail.cpp**
   - Added sampling angle printing
   - Added edge angle error printing for sample objects

2. **src/detect_3d_cuboid/object_3d_util.cpp**
   - Added angle difference printing in `box_edge_alignment_angle_error` function
   - Added total angle error printing

## How to Use

When you run the EAO-SLAM system with object detection enabled, these print statements will automatically output the angles to the console (stdout). The angles are converted from radians to degrees for easier interpretation.

## Notes

- All angles are converted from radians to degrees for printing
- A `print_count` counter is used in the angle error calculation function to limit output to the first 3 calls
- The sample object printing is limited to the first 10 objects using a static counter
- These print statements help verify the implementation matches the paper's description
