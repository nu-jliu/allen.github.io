---
layout: project
title: Omnid Mocobots
description: C++, Python, ROS 2, Machine Learning
image: assets/images/omnid-mocobots/main.gif
imagewidth: 0
order: 988
---

For my final project in Northwestern University's MSR program, I am working with the unique [**omnidirectional mobile cobots**](https://www.mccormick.northwestern.edu/news/articles/2022/08/mobile-cobots-offer-glimpse-of-future-of-human-robot-interaction/), dubbed "omnid mocobots" or "omnids" for short. These collaborative robots were developed at Northwestern University by [**Elwin et al.**](https://arxiv.org/abs/2206.14293).

{% include youtube.html video_id="b-YfUltluHQ" width="75%" %}

****

## ROS 2 Conversion

The first part of this project was to convert the omnids from running ROS 1 Noetic to running ROS 2 Humble, so their development could continue as ROS 1 is phased out. I worked on this process with my peer [Hang-Yin](https://hang-yin.github.io/portfolio/) and our advisor, [Matthew Elwin](https://robotics.northwestern.edu/people/profiles/faculty/elwin-matt.html).

This process involved installing Ubuntu 22.04 on the omnids' Intel NUCs and upgrading a series of ROS packages to Humble. Since - at the time of this work - ROS 2 had yet to achieve full feature parity with ROS 1, I had to implement several features that were already available in ROS 1. For example, I extended [ROS 2's launch system](https://github.com/ros2/launch) to allow **remotely launching launch files** on a computer from another. I also wrote and released an open source ROS package for **simple C++ integration and unit testing** using [Catch2](https://github.com/catchorg/Catch2) - more on that [here](TODO).

We chose to upgrade to Humble first, since it's currently the newest LTS ROS 2 distro, but we will very soon be upgrading the platform to ROS 2 Iron to take advantage of its improvements over Humble.

## Future Work

Now that the conversion is completed, I'll be beginning to use the omnid research platform to implement an imitation learning pipeline to teach the collaborative robots tasks. More on these efforts to come!