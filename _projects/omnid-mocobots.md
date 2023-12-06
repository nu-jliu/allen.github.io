---
layout: project
title: Omnid Mocobots
description: C++, Python, ROS 2
image: assets/images/omnid-mocobots/main.gif
imagewidth: 0
order: 991
---

For my final project in Northwestern University's MSR program, I worked with the unique [**omnidirectional mobile cobots**](https://www.mccormick.northwestern.edu/news/articles/2022/08/mobile-cobots-offer-glimpse-of-future-of-human-robot-interaction/), dubbed "omnid mocobots" or "omnids" for short. These collaborative robots were developed at Northwestern University by [**Elwin et al.**](https://arxiv.org/abs/2206.14293).

The main focus of this project was to research [novel ways to apply assistive action prediction](/projects/diffusion-policy-assistive-action-prediction) to these robots with generative machine learning models, but I also did plenty of work **improving their onboard systems**. This post goes into more detail about those efforts.

{% include youtube.html video_id="b-YfUltluHQ" width="75%" %}

****

## ROS 2 Conversion

The first part of this project was to convert the omnids from running ROS 1 Noetic to running ROS 2 Iron, so their development could continue as ROS 1 is phased out. I worked on this process with my peer [Hang-Yin](https://hang-yin.github.io/portfolio/) and our advisor, [Matthew Elwin](https://robotics.northwestern.edu/people/profiles/faculty/elwin-matt.html).

This process involved installing Ubuntu 22.04 on the omnids' Intel NUCs and upgrading a series of ROS packages to Iron. Since - at the time of this work - ROS 2 has yet to achieve full feature parity with ROS 1, I had to implement several features that were already available in ROS 1.

### catch_ros2
The omnid software stack previously made heavy use of the ROS 1 [`catch_ros`](https://github.com/AIS-Bonn/catch_ros) package for unit and integration testing. This package was never upgraded to ROS 2, so there was not a simple path to update these tests directly.

Motivated by this and a desire to introduce a **simple, native C++ unit/integration testing framework for ROS 2**, I wrote and released an open source ROS package. `catch_ros2` is now available as Debian packages in several ROS distros - read more [here](/projects/catch_ros2).

### launch_remote_ssh
ROS 1 provided the capability to launch nodes from a launch file on one machine remotely on another machine. This capability has been widely discussed, but never implemented in ROS 2. As of the time of writing this post, the design document (not the implementation) for this feature remains an [open PR](https://github.com/ros2/design/pull/297) on the ROS 2 design repository, with the last discussion in February of 2021.

The omnid system relied heavily on ROS 1's remote launch capabilities to coordinate the launching of robot-specific nodes on each of the three omnids from a separate machine (the "station"). In order to mimic this performance in ROS 2, I found a [workaround](https://answers.ros.org/question/364152/remotely-launch-nodes-in-ros2/) that creates a screen session, SSH's into the remote system, and launches the nodes. As long as SSH keys are set up between the machines, this works reliably and allows users to read output from nodes on the remote machine.

I formalized this workaround into the [`launch_remote_ssh`](https://github.com/NU-MSR/launch_remote), which focuses on the ability to remotely launch launch files. There is also limited support for remotely running nodes and processes. The package extends the [ROS 2 launch system](https://github.com/ros2/launch), providing support for launching remotely in both Python and frontend (XML/YAML) launch files.

Another unique capability is the concept of "flexible" launch files. Flexible launch files can be launched locally (default) like a normal ROS 2 launch file. They can also be launched remotely from the local machine on another machine by simply specifying the `flexible_launch.user` and `flexible_launch.machine` arguments. This is a convenient way to be able to run the same launch file either locally or remotely by just changing a few parameters. Flexible launch files can be generated using a CMake function which converts launch files marked as "core" launch files into flexible launch files. Currently only XML launch files are supported for this.

As of the time of writing this post, the `launch_remote_ssh` package is a work in progress. I plan to eventually release it as a ROS Debian package. If you'd like to try it out or contribute, please do so!