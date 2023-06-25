---
layout: project
title: OpenCV-Powered Pen Thief
description: Python, OpenCV, PincherX 100
image: assets/images/pen-thief/main.gif
imagewidth: 0
order: 995
---

[GitHub Repository](https://github.com/ngmor/pen-challenge)

As a start to the MS in Robotics program at Northwestern University, my fellow cohort members and I participated in an orientation/hackathon in which we learned about several essential topics in the world of robotics. The hackathon culminated with a challenge to use **computer vision** and a **robotic arm** to reach out and snatch a Northwestern-purple pen from an unsuspecting human's (or robot's) grasp.

To complete this challenge, I used an [**Intel RealSense D435i**](https://www.intelrealsense.com/depth-camera-d435i/) along with the **PyRealSense** and **OpenCV** libraries to detect the pen in the robot's workspace via its color. Then I used a [**Trossen PincherX-100 Robot Arm**](https://www.trossenrobotics.com/docs/interbotix_xsarms/specifications/px100.html) with its Python API and the [**Modern Robotics**](https://github.com/NxRLab/ModernRobotics) library to grab the pen.


{% include youtube.html video_id="pJKTEZ_dBF0" width="75%" %}

<br>

After successfully completing the challenge, a few of my fellow cohort members and I programmed our robots to hand the pen off to eachother in a relay as shown in the video!

### How It Worked

The D435i provides both color and depth information. To find the pen's location, I could determine the pixels representing the pen in the image by thresholding the color image based on a range of HSV values which represented the pen's color. Then I could find a contour around these pixels and find the 2D coordinates of the centroid of that contour in the image. Finally, the depth data from the D435i gave the last coordinate needed to describe the pen's 3D coordinates in the camera's reference frame.

{:refdef: style="text-align: center;"}
![Possible total height reduction](/assets/images/pen-thief/pen-tracking.gif){: width="50%"}
{: refdef}

To determine the location of the pen in the robot's reference frame, I added a calibration routine. In this routine, I held the pen up to the robot's gripper. I then used forward kinematics to determine the position of the robot's gripper in the robot's frame. I simplified the calibration by assuming that the camera was oriented horizontally and at a 90° angle to the robot arm. With that assumption, a comparison of the camera's pen coordinates and the robot's gripper position allowed me to determine a transformation between the camera and the robot reference frames.

Knowing this information, I could then determine the location of the pen in the robot's reference frame regardless of where I placed it. From there, a simple algorithm adjusted the PincherX 100's waist angle and executed a trajectory to move the gripper to the pen. Once it was within a tolerance, the gripper closed on the pen.

### Possible Improvements
I always like to think of how I could improve my projects if given a next iteration! Here are some of my ideas:

{% details **1. Improve the calibration method.** %}

The calibration method to determine the transformation between the camera reference frame and the robot reference frame relies on an assumption about the orientation of the camera relative to the robot. An improvement on this might be to use some sort of **[fiducial marker](https://en.wikipedia.org/wiki/Fiducial_marker)**, like an AprilTag affixed to the robot's base. By choosing a known geometry between the robot's reference frame and the location of the marker, I could detect the marker in my CV application and calculate a much more accurate transformation.

{% enddetails %}

{% details **2. Add communication during the relay race.** %}

Due to time limitations of the hackathon, our pen thief relay race relied on timing for the robots to know when to act in relation to one another. This is of course very unreliable. A much better solution would be to have the robots actually communicate to one another to decide when each robot would grasp, release, and move the pen. We could accomplish this by taking advantage of the [**socket**](https://docs.python.org/3/library/socket.html) library in Python to send simple **TCP packets** between the robots for sequence handshakes.

{% enddetails %}