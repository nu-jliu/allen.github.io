---
layout: project
title:  Pick and Place with Ridheback-Sawyer-PX100 Robot
description: ROS1, Linux, Bash Shell, C++, Python, MoveIt!, Navigation, Rethink Sawyer Robot Arm, Interbotix PincherX-100 Robot Arm, Clearpath Ridgeback Mobile Platform
image: assets/images/sawback.gif
# featured: true
# hidden: true
imagewidth: 50%
order: 11
---


<!-- ROS1, ROS Noetic, Linux, Bash Shell, C++, Python, Moveit!, Navigation, Rethink Sawyer Robot Arm, Interbotix PincherX-100 Robot Arm, Clearpath Ridgeback Mobile Platform -->

[View This Project on GitHub](https://github.com/nu-jliu/Winter_Project.git)

[Sawyer MoveIt!](https://github.com/nu-jliu/sawyer_moveit/tree/allen/Winter_project)

[Intera SDK](https://github.com/nu-jliu/intera_sdk/tree/allen/Winter_Project)

# Description
This project aims for implementing a `pick-and-place` pipline on a mobile manipulator via `ROS Noetic`, which is a `Clearpath Ridgeback Mobile Platform` with `Rethink Sawyer Robot Arm` and `Interbotix PincherX-100 Robot Arm mounted on top of it`.

<!-- `TODO: Add A Figure` -->

![](https://github.com/nu-jliu/Winter_Project/blob/main/sawback.png?raw=true)

# Structure
The overall detailed structure of the syste, is shown in the figure below, which can be divided into 3 subsystems.
 - `Robot Arm Control`: Used for controlling the motion of the `Sawyer` robot arm given a goal pose command
 - `Mobile Platform Control`: Used for controlling the motion of the `Ridgeback` mobile platform.
 - `Command Interface`: Retriving the goal from user, and parsing it into the command given to the arm and base controller, so it will move to the goal as intended.

![](/assets/images/sawback_system.png)

## Software

### Pick
On `Pick`, user will first select the target to pick and then the `ridgeback` will move to the target pose and `sawyer` will pick up the target.

![](/assets/images/pick_sawback.png)

### Place

![](/assets/images/place.drawio.png)

## Hardware
The overall hardware structure is shown in the figure below, which is a sawyer arm and a px100 arm with a realsense cammera attached mounted on the top of the ridgeback. 

## Network
The network configureation of the overall system is shown in the figure below

![](/assets/images/network.png)

This system consists of 3 main machine:
 - `PC`: The user control interface for user to send the command to the system.
 - `Ridgeback Computer`: Used for perform all calculations given command sent from user and control the motion of the platform.
 - `Sawyer Computer`: Used for control the motion of the sawyer arm given the command from ridgeback computer.

## ROS
Based on the `ROS1` standard, in order for all nodes to be discovered by others, the `ROS_MASTER_URI` must be set to be the same, so that all nodes are running under the same `ROS master`. So that all parameters and topics are served on the same server so that they can listen to others.

### Packages
 - `motion_control`: Used the `ROS Navigation` to control the motion of the mobile platform.
 - `arm_control`: ontrol the motion of the robot arm.
 - `vision_control`: Calibrate the camera and control the motion of the `px100` arm.
 - `object_detection`: Used for detect target pose as for the target object.
 - `picker_interfaces`: Customized interfaces for transporting data between `ROS` nodes
 - `manipulator_description`: Visualizing the robot over `rviz`.


<!-- ## Frames -->

# Final Result

<iframe width="560" height="315" src="https://www.youtube.com/embed/G6C1wfTlVRs?si=QuOxMdutTlqjoj_H&amp;controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<!-- `TODO:` -->

# Technical Challenges

## `Moveit!` with Sawyer
The default `Moveit!` interface configuration for `sawyer` was not compatible with the current configuration of the robot since the robot arm is mounted on the base instead of mounting on the control algorith coming along with the software.

<!-- ## ROS Navigation -->


## Sawyer Hardware
There was a lot of issue setting up the `Rethink Sawyer` robot arm, there was some both mechanical and software issue for fixing it. 

Link: [Fixing Sawyer Arm](https://nu-jliu.github.io/sawyer/)

## Low Bandwidth
The robot is configured to use a 2G wifi to communicate with the laptop, making it difficult to transport large amount of data within short period of time. Which increases the latency between sensor and when data is processed. This will cause inacuracy on SLAM, path planning and motion planning. 