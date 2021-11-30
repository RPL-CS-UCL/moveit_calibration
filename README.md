# RPL Custom Instructions

## Setup

From a ROS workspace that has the robot's rviz bringup do the following

```bash
cd src
git clone <this repo>
git clone https://github.com/PickNikRobotics/rviz_visual_tools.git
cd .. 
rosdep install --from-paths src --ignore-src --rosdistro melodic
catkin build
```

## Calibration

- 1) Run your rviz bring up, e.g. `roslaunch rpl_panda_with_rs display_real.launch cam_on_hand:=false cam_on_scene:=true`
**IMPORTANT! Do not publish any transform between robot and the camera link**
- 2) RVIZ > Add > Handeye calibration 
- 3) Select ChAruco setup, print out, measure the long side and the marker side (I think the marker side is the small squares, not 100% sure)
- 4) Fill out all the setuo fields. Make sure you select your `camera_link` for the Sensor link! If you select the optical or color frame, it will ruin the calibration and you have to start iv) over.
- 5) Add the TF's and Camera display in RVIZ
- 6) Calibrate!
- 6.1) If hand-on-eye, ChAruco on the ground, if hand-to-eye, ChAruco grasped by end-effector. 
- 6.2) Move the robot in diffferent positions such that the ChAruco board is visible. 
- 6.3) Take sample at each pose, making sure that the handeye_target TF is displayed at the correct place in the camera display!
- 7) Voila! Save the camera_pose (it's a launch file, save wherever you need it and launch it with the rest of the robot)

Tested on Melodic with D455 for external camera calibration. Got 0.01 rad orientation error and 0.04m position error.

TODO:
- [ ] Check if OpenCV is making it less accurate. Suggest how to install a better OpenCV version without breaking ROS.
- [ ] Check if marker_size is the Aruco Tag or the black box side.

The following is the original repo description, but you can ignore it:

# MoveIt Calibration

*Tools for robot arm hand-eye calibration.*

| **Warning to Melodic users** |
| --- |
| OpenCV 3.2, which is the version in Ubuntu 18.04, has a buggy ArUco board pose detector. Do not expect adequate results if you are using an ArUco board with OpenCV 3.2. |

MoveIt Calibration supports ArUco boards and ChArUco boards as calibration targets. Experiments have demonstrated that a
ChArUco board gives more accurate results, so it is recommended.

This repository has been developed and tested on ROS Melodic and Noetic. It has not been tested on earlier ROS versions.
When building `moveit_calibration` on ROS Melodic, `rviz_visual_tools` must also be built from source.

This package was originally developed by Dr. Yu Yan at Intel, and was originally submitted as a PR to the core MoveIt
repository. For background, see this [Github discussion](https://github.com/ros-planning/moveit/issues/1070).

## GitHub Actions - Continuous Integration

[![Format](https://github.com/ros-planning/moveit_calibration/actions/workflows/format.yaml/badge.svg?branch=master)](https://github.com/ros-planning/moveit_calibration/actions/workflows/format.yaml?branch=master)
[![BuildAndTest](https://github.com/ros-planning/moveit_calibration/actions/workflows/ci.yaml/badge.svg?branch=master)](https://github.com/ros-planning/moveit_calibration/actions/workflows/ci.yaml?branch=master)
[![codecov](https://codecov.io/gh/ros-planning/moveit_calibration/branch/master/graph/badge.svg?token=W7uHKcY0ly)](https://codecov.io/gh/ros-planning/moveit_calibration)
