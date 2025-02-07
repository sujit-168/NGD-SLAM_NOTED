# NGD-SLAM_NOTED

# Acknowledgement

This project is based on the [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3) framework and [NGD-SLAM](https://github.com/yuhaozhang7/NGD-SLAM), Thanks to the authors of the above projects，This project merged the main branch code of the NGD-SLAM project based on the master branch of ORB_SLAM3 to facilitate finding the modifications and subsequent annotations and understanding.

# Brief Introduction
- Paper: arXiv:2405.07392
- Code: https://github.com/yuhaozhang7/NGD-SLAM

Use the previous segmentation results to quickly generate a mask for the dynamic object in the current frame, so that the tracking thread does not have to wait for the output of the neural network.
Mix optical flow and ORB features for static key point tracking.
The deep model is directly read and inferred using OpenCV, and the environment configuration is exactly the same as ORB-SLAM3 (tested on Ubuntu20.04 and 22.04).

This is a visual SLAM system designed for dynamic environments, based on the [ORB-SLAM3](https://github.com/UZ-SLAMLab/ORB_SLAM3) framework. It runs in real-time on a single laptop CPU without compromising tracking accuracy [[Demo](https://www.bilibili.com/video/BV1XKT5eaEsT/)] [[Paper](https://arxiv.org/abs/2405.07392)].

# 1. Prerequisites
The system is tested on **Ubuntu 20.04** and **22.04**, and it should be easy to compile in other platforms. A powerful computer will provide more stable and accurate results.

## C++11 or C++0x Compiler
It uses the new thread and chrono functionalities of C++11.

## Pangolin
It uses [Pangolin](https://github.com/stevenlovegrove/Pangolin) for visualization and user interface. Dowload and install instructions can be found at: https://github.com/stevenlovegrove/Pangolin.

## OpenCV
It uses [OpenCV](http://opencv.org) to manipulate images and features. Dowload and install instructions can be found at: http://opencv.org. **Required at leat 4.4**.

## Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. **Required at least 3.1.0**.

## YOLO (Included in Thirdparty folder)
It uses the [C++ version](https://github.com/hpc203/yolov34-cpp-opencv-dnn) of the [YOLO-fastest](https://github.com/dog-qiuqiu/Yolo-Fastest.git) model. The model configuration and pre-trained weights are included in the *Thirdparty* folder and are loaded using OpenCV.

## DBoW2 and g2o (Included in Thirdparty folder)
It uses modified versions of the [DBoW2](https://github.com/dorian3d/DBoW2) library to perform place recognition and [g2o](https://github.com/RainerKuemmerle/g2o) library to perform non-linear optimizations. Both modified libraries (which are BSD) are included in the *Thirdparty* folder.

## Python
Required to calculate the alignment of the trajectory with the ground truth. **Required Numpy module**.

* (win) http://www.python.org/downloads/windows
* (deb) `sudo apt install libpython2.7-dev`
* (mac) preinstalled with osx

# 2. Building NGD-SLAM
Clone the repository:
```
git clone https://github.com/yuhaozhang7/NGD-SLAM.git NGD-SLAM
```

It provides a script `build.sh` to build the libraries. Please make sure you have installed all required dependencies. Execute:
```
cd NGD-SLAM_NOTED
chmod +x build.sh
./build.sh
```

# 3. Running TUM Examples
[TUM dataset](https://cvg.cit.tum.de/data/datasets/rgbd-dataset/download) includes sequences that are captured using an RGB-D camera in dynamic environments. Download the desired sequence and uncompress it. Below is an example command for the *freiburg3_walking_xyz* sequence:
```
./Examples/RGB-D/rgbd_tum ./Vocabulary/ORBvoc.txt ./Examples/RGB-D/TUM3.yaml ./path/to/TUM/rgbd_dataset_freiburg3_walking_xyz ./Examples/RGB-D/associations/fr3_walk_xyz.txt
```