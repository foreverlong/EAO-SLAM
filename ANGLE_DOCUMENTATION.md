# 论文中的夹角 (Angle in the Paper)

## 找到的夹角 (Found Angle)

在 EAO-SLAM 论文的实现中，主要的夹角阈值是：

**5度 (5°)** - 用于线段合并的角度阈值

## 位置 (Location)

这个夹角阈值在代码中的三个位置被使用：

### 1. src/Tracking.cc (第2514行)
```cpp
double pre_merge_angle_thre = 5;  // angle threshold between two line, 5°.
```

用途：在目标跟踪过程中，合并检测到的线段。

### 2. src/detect_3d_cuboid/box_proposal_detail.cpp (第214行)
```cpp
double pre_merge_angle_thre = 5;
```

用途：在3D立方体检测过程中，合并目标内部的边缘线。

### 3. src/detect_3d_cuboid/object_3d_util.cpp (第352-359行)
```cpp
double pre_merge_angle_thre_degree,  // 参数传入（度）
...
double pre_merge_angle_thre = pre_merge_angle_thre_degree/180.0*M_PI;  // 转换为弧度
```

用途：`merge_break_lines` 函数的实现，将度数转换为弧度进行计算。

## 功能说明 (Functionality)

**角度阈值的作用 (Purpose of Angle Threshold):**

这个5度的角度阈值用于判断两条线段是否可以合并。在 `merge_break_lines` 函数中：
- 如果两条线段之间的角度差小于5度
- 并且满足其他条件（距离、长度等）
- 这两条线段会被合并成一条

**相关参数 (Related Parameters):**
- `pre_merge_dist_thre = 20` (像素) - 两条线之间的距离阈值
- `pre_merge_angle_thre = 5` (度) - 两条线之间的角度阈值
- `edge_length_threshold = 30` (像素) - 线段长度阈值

## 论文引用 (Paper Reference)

这个角度阈值在以下论文中被使用：

Wu Y, Zhang Y, Zhu D, et al. **EAO-SLAM: Monocular Semi-Dense Object SLAM Based on Ensemble Data Association**. 2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2020: 4966-4973.

## 相关函数 (Related Functions)

- `merge_break_lines()` - 合并断裂的线段
  - 定义于: `include/detect_3d_cuboid/object_3d_util.h`
  - 实现于: `src/detect_3d_cuboid/object_3d_util.cpp`
