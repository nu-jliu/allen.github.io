---
layout: project
title: Nauticus Robotics 3D Path Planner
description: C++, ROS 2, Navigation, Underwater Robotics
image: assets/images/nauticus/main.gif
imagewidth: 0
order: 988
---

During the summer before my final quarter in Northwestern University's MSR program, I worked on a 3D path planner for use in **Wayfinder**, a part of the [**toolKITT**](https://nauticusrobotics.com/toolkitt/) software suite created by [**Nauticus Robotics**](https://nauticusrobotics.com/). The goal of the planner was to guide autonomous underwater vehicles (AUVs) like the [Aquanaut MK2](https://nauticusrobotics.com/aquanaut/) along near-optimal paths through dynamically changing three dimensional underwater environments.

Here's a video of my path planner in action!

{% include youtube.html video_id="AcLWpmjOrFE" width="75%" %}

<br>

The planner pipeline was based on the [Development of a Three Dimensional Path Planner for Aerial Robotic Workers](https://essay.utwente.nl/71490/1/ZOMPAS_MA_EWI.pdf) by Anastasios Zompas, which uses the concept of an octomap-based visibility graph to efficiently plan near-optimal paths through a sparse environment. While working on the Nauticus software team, I implemented the paper's pipeline and added several proprietary improvements and features to the base concept. These included improving the visibility graph construction times drastically through several different methods, adding collision detection and replanning capabilities, and integrating the planner into Nauticus's existing navigation suite.

This was an incredible learning experience as I delved deeply into techniques for enhancing software performance. I worked with several new data structures and concepts and learned how and when to apply each to leverage the best possible performance and robustness. I also employed behavior trees - a high-level logic control structure I had not previously used - to demonstrate the usage of the planner in simulation.

Overall, the project was a great experience and I'm looking forward to working on similar projects in the future!