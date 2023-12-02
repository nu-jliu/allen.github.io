---
layout: project
title: catch_ros2
description: ROS 2 C++ Integration/Unit Testing
image: assets/images/catch_ros2/main.png
imagewidth: 0
order: 992
---

{:refdef: style="text-align: center;"}
![catch_ros2](/assets/images/catch_ros2/main_cropped.png)
{: refdef}

[**`catch_ros2`**](https://index.ros.org/p/catch_ros2/#rolling) is an open source ROS 2 package that I developed as part of my work on the [omnid mocobots](/projects/omnid-mocobots). It fills a gap in the ROS 2 ecosystem for a C++ testing framework that allows simple setup of both **unit testing** (for library classes, functions, etc.) and **integration testing** (for testing the interactions between ROS nodes). 

[Catch2](https://github.com/catchorg/Catch2) is a very simple and easy to use C++ testing framework. At the time of this work, Ubuntu 22.04 (the LTS Linux distro ROS 2 uses) only provides Catch v1 and Catch2 v2 - not the most updated version, Catch2 v3. The `catch_ros2` package vendors Catch2 v3 so the most updated version can be used for testing in ROS 2 projects.

ROS 2 already contains a [framework for writing integration tests in Python](https://github.com/ros2/launch/tree/rolling/launch_testing) which may be more than sufficient for most users. GTest can also be used to test C++ code within an integration test. However, to my knowledge, I have not seen an easy way to create integration tests in C++ with Catch2. The `catch_ros2` package aims to fill that gap.

This package is available for ROS 2 [Humble](https://index.ros.org/p/catch_ros2/#humble), [Iron](https://index.ros.org/p/catch_ros2/#iron), and [Rolling](https://index.ros.org/p/catch_ros2/#rolling) as Debian packages. Head over to the linked ROS index pages for links to the source repository (which has documentation) and see if this package could help your project!