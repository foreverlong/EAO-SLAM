# EAO-SLAM

**Related Paper:**  

+ Wu Y, Zhang Y, Zhu D, et al. **EAO-SLAM: Monocular Semi-Dense Object SLAM Based on Ensemble Data Association**[C]//2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). IEEE, 2020: 4966-4973. [[**Paper**](https://ieeexplore.ieee.org/abstract/document/9341757)] [[**Arxiv**](https://arxiv.org/abs/2004.12730)] [[**YouTube**](https://youtu.be/pvwdQoV1KBI)] [[**bilibili**](https://www.bilibili.com/video/av94805216)]  [[**Project page**](https://yanmin-wu.github.io/project/eaoslam/)].
+ Extended Work
    + Wu Y, Zhang Y, Zhu D, et al. **Object SLAM-Based Active Mapping and Robotic Grasping**[C]//2021 International Conference on 3D Vision (3DV). IEEE, 2021: 1372-1381. [[**Paper**](https://ieeexplore.ieee.org/document/9665905)] [[**Arxiv**](https://arxiv.org/abs/2012.01788)] [[**Project page**](https://yanmin-wu.github.io/project/active-mapping/)].
    + Robotic Grasping demo: [YouTube](https://youtu.be/cNtvqiArVfI) | [bilibili](https://www.bilibili.com/video/BV1ZA411p7KK)
    + Augmented Reality demo: [YouTube](https://youtu.be/E8jfkO_Q7Iw) | [bilibili](https://www.bilibili.com/video/BV1V5411p7gA)
+ If you use the code in your academic work, please cite the above paper. 

---

## 1. Prerequisites

+ Prerequisites are the same as [**semidense-lines**](https://github.com/shidahe/semidense-lines#1-prerequisites). If compiling problems met, please refer to semidense-lines and ORB_SLAM2.
+ The code is tested in Ubuntu 16.04, opencv 3.2.0/3.3.1, Eigen 3.2.1.

## 2. Building

```
chmod +x build.sh       
./build.sh
```

## 3. Examples

+ **3.0** We provide a demo that uses the **`TUM rgbd_dataset_freiburg3_long_office_household`** sequence;  please download the dataset beforehand. The offline object bounding boxes are in `data/yolo_txts` folder.
+ **3.1** **Object size and orientation estimation**.
    + use **iForest and line alignment**:
        ```
        ./Examples/Monocular/mono_tum LineAndiForest [path of tum fr3_long_office]
        ```
    + only use **iForest**:
        ```
        ./Examples/Monocular/mono_tum iForest [path of tum fr3_long_office]
        ```
    + **without** iForest and line alignment:
        ```
        ./Examples/Monocular/mono_tum None [path of tum fr3_long_office]
        ```
    <figure>
    <p align="center" >
    <img src='./figures/scale_and_orientation.png' width=1000 alt="Figure 1"/>
    </p>
    </figure>

+ **3.2** **Data association**
    + **without** data association:
        ```
        ./Examples/Monocular/mono_tum NA [path of tum fr3_long_office]
        ```
    + data association by **IoU** only:
        ```
        ./Examples/Monocular/mono_tum IoU [path of tum fr3_long_office]
        ```
    + data association by **Non-Parametric-test** only:
        ```
        ./Examples/Monocular/mono_tum NP [path of tum fr3_long_office]
        ```
    + data association by our **ensemble method**:
        ```
        ./Examples/Monocular/mono_tum EAO [path of tum fr3_long_office]
        ```
    <figure>
    <p align="center" >
    <img src='./figures/data_association.jpg' width=1000 alt="Figure 1"/>
    </p>
    </figure>

+ **3.3** **The full demo on TUM fr3_long_office sequence:**
    ```
    ./Examples/Monocular/mono_tum Full [path of tum fr3_long_office]
    ```
    + If you want to see the semi-dense map, you may have to wait a while after the sequence ends.
    + Since YOLO (which was not trained in this scenario) made a lot of false detections at the start of the sequence, so we adopted a stricter elimination mechanism, which resulted in the deletion of many objects at the start.

## 4. Videos

+ More experimental results can be found on our [project page](https://yanmin-wu.github.io/project/eaoslam/).   
    + Video: [**YouTube**](https://youtu.be/pvwdQoV1KBI) | [**bilibili**](https://www.bilibili.com/video/av94805216)
+ Extended work: [project page](https://yanmin-wu.github.io/project/active-mapping/)
    + Robotic Grasping demo: [YouTube](https://youtu.be/cNtvqiArVfI) | [bilibili](https://www.bilibili.com/video/BV1ZA411p7KK)
    + Augmented Reality demo: [YouTube](https://youtu.be/E8jfkO_Q7Iw) | [bilibili](https://www.bilibili.com/video/BV1V5411p7gA)

## 5. Improvements

### 5.1 Variable Sampling Angle Strategy for Object Orientation Estimation

This improvement enhances the object orientation estimation by implementing an adaptive sampling strategy:

**Key Features:**
+ **Variable Step Size**: Instead of fixed 3° sampling, uses adaptive 1°-4° steps based on error feedback
+ **Two-Phase Sampling**:
  - Phase 1: Coarse sampling with 4° step across -44° to 44° range (23 samples, symmetric around 0°)
  - Phase 2: Adaptive refinement around promising angles with 1-3° steps
+ **Error-Based Adaptation**: Step size adapts based on previous frame's angle error:
  - Low error (< 0.1) → 1° fine steps for precision
  - High error (> 0.5) → 3° steps for broader search
+ **Benefits**: Achieves better orientation accuracy with comparable computational cost

**Implementation Details:**
+ Modified `SampleObjYaw()` function in `src/Tracking.cc`
+ Uses `mvAngleTimesAndScore` history to guide adaptive sampling
+ Refines around top-3 candidate angles when their scores are competitive

## 6. Note

+ This is an incomplete version of our paper. If you want to use it in your work or with other datasets, you should prepare the offline semantic detection/segmentation results or switch to online mode. Besides, you may need to adjust the data association strategy and abnormal object elimination mechanism (We found the misdetection from YOLO has a great impact on the results).

## 7. Acknowledgement

Thanks for the great work: [**ORB-SLAM2**](https://github.com/raulmur/ORB_SLAM2), [**Cube SLAM**](https://github.com/shichaoy/cube_slam), and [**Semidense-Lines**](https://github.com/shidahe/semidense-lines).
+ Mur-Artal R, Tardós J D. **Orb-slam2: An open-source slam system for monocular, stereo, and rgb-d cameras**[J]. IEEE Transactions on Robotics, 2017, 33(5): 1255-1262. [PDF](https://arxiv.org/abs/1610.06475), [Code](https://github.com/raulmur/ORB_SLAM2)
+ Yang S, Scherer S. **Cubeslam: Monocular 3-d object slam**[J]. IEEE Transactions on Robotics, 2019, 35(4): 925-938. [PDF](https://arxiv.org/abs/1806.00557), [Code](https://github.com/shichaoy/cube_slam)
+ He S, Qin X, Zhang Z, et al. **Incremental 3d line segment extraction from semi-dense slam**[C]//2018 24th International Conference on Pattern Recognition (ICPR). IEEE, 2018: 1658-1663. [PDF](https://arxiv.org/abs/1708.03275), [Code](https://github.com/shidahe/semidense-lines)

## 8. Contact

+ [Yanmin Wu](https://yanmin-wu.github.io/), Email: wuyanminmax@gmail.com
+ Corresponding author: [Yunzhou Zhang *](http://faculty.neu.edu.cn/ise/zhangyunzhou), Email: zhangyunzhou@mail.neu.edu.cn

```
@inproceedings{wu2020eao,
  title={EAO-SLAM: Monocular semi-dense object SLAM based on ensemble data association},
  author={Wu, Yanmin and Zhang, Yunzhou and Zhu, Delong and Feng, Yonghui and Coleman, Sonya and Kerr, Dermot},
  booktitle={2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4966--4973},
  year={2020},
  organization={IEEE}
}

@inproceedings{wu2021object,
  title={Object SLAM-Based Active Mapping and Robotic Grasping},
  author={Wu, Yanmin and Zhang, Yunzhou and Zhu, Delong and Chen, Xin and Coleman, Sonya and Sun, Wenkai and Hu, Xinggang and Deng, Zhiqiang},
  booktitle={2021 International Conference on 3D Vision (3DV)},
  pages={1372-1381},
  year={2021},
  organization={IEEE}
}
```
