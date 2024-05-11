---
layout: project
title:  Pen Theif
description: Python, OpenCV, ROS2, MoveIt2, Interbotix Pincher PX100
image: assets/images/pen.gif
# featured: false
# hidden: false
imagewidth: 50%
order: 21
---

<!-- Python, Computer Vision, ROS2, MoveIt2, Interbotix Pincher PX100 -->

**Authors**: Allen Liu

**GitHub**: [View This Project on GitHub](https://github.com/nu-jliu/hackathon_Pen)

# Project Description
Used the `OpenCV` to detect the location of a pen within a camera frame, and then send the command to the `Interbotix Pincher PX100` robot arm to grab the pen and drop it at another place.

# Structure
## Computer Vision
 - Used the python `OpenCV` packages to applie a color filter in HSV scheme so that it blacks out all other colors.
 - Applied the depth sensor reading into the filtered image to get the pen location within the camera frame.

## Robot Comtrol
 - Applied the transform from the camera to robot base to get the pen location relative to the robot frame.
 - Calls the official control API from `Interbotix` to command the robot arm to go to the pen location, grab it and return it back to home pose.

# Robot in Action

<iframe width="560" height="315" src="https://www.youtube.com/embed/0IB5zzDqvyM?si=jeceXWgKSJuJdSA3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>