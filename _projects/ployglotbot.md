---
layout: project
title:  Polyglot Translator Robot
description: OCR, YOLO, Python, ROS2, MoveIt2, Emika Franka Robot Arm
image: assets/images/polyglotbot.gif
# featured: true
# hidden: true
imagewidth: 50%
order: 12
---

<!-- OCR, YOLO, Python, ROS2, MoveIt2, Emika Franka Robot Arm -->

**Authors**: Allen Liu, Damien Koh, Kassidy Shedd, Henry Brown, Megan Black

**GitHub**: [View this project on GitHub](https://github.com/nu-jliu/final-project-me495)

# Project Description
Lead the team in creating a sophisticated system that can process input from either text written on a whiteboard or audio received through a microphone. This ambitious project involves multiple stages, including text and speech recognition, language identification, translation into the target language, and the unique aspect of controlling a robot arm to physically write the translated text onto a whiteboard. The team will need to implement strong text and speech recognition capabilities, incorporating language detection mechanisms for accurate translation. The integration of a robot arm introduces a novel challenge, requiring seamless communication between language processing components and the robotic control system. Collaboration across domains such as natural language processing, machine learning, computer vision, and robotics is essential for success. Emphasize usability, accuracy, and real-time responsiveness throughout the development process, with regular testing and iterative improvements to refine the system's performance. As the team leader, encourage a creative and problem-solving mindset, fostering effective communication and coordination to bring together the diverse components into a cohesive and functional system.

# Architecture
This project consists of 5 subsystems, in which each group member is in charge of one of them:
 - `writer`: Allen was in chage of this subsystem, which uses the `cartesian path planner` from `moveit2` package to find and execute the path for the robot to write the specific characters on the write board, while calibrating the relative location of the whiteboard using the apriltags. 
 - `translation`: Damine was in charge of this subsystem, which calls `google-translation` API for translating from the source language to the target language.
 - `computer_vision`: Megan was in charge of this subsystem, which uses the `YOLO` and `ocr` python package for recongnizing texts and human detection.
 - `string2waypoints`: Kassidy as in charge of this subsystem, which uses the `matplotlib` python package to generate the waypoints for robot to travel through.
 - `apriltags`: Henry was in charge of this subsystem, which uses the `apriltag_ros` package for detecting the location and orientation of each apriltag, used for pinpointing the location and orientation of the whiteboard. 

# Features

## Translate from Chinese to English
<iframe width="560" height="315" src="https://www.youtube.com/embed/EdNptTr9Y0U?si=ZA9z5NeRokLx9CQw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Translate from German to French
<iframe width="560" height="315" src="https://www.youtube.com/embed/iFoNAHWQ9wE?si=es4q5_6K3KzxVzFM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Translate from Spanish to Korean
<iframe width="560" height="315" src="https://www.youtube.com/embed/Qjd3jGlU8Ds?si=3aTC0FWY6vf4mSaL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Translate from Simpified Chinese to Traditional Chinese 
<iframe width="560" height="315" src="https://www.youtube.com/embed/9GdGPy74Qwg?si=FwCUxvQhKl4Oi-Fo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Hindi Voice to English
<iframe width="560" height="315" src="https://www.youtube.com/embed/BeQzYUYSe5k?si=6rlJjthlDPSmCw-F" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Spanish Voice to English
<iframe width="560" height="315" src="https://www.youtube.com/embed/9PCf_-gbIbU?si=YZCxg8eNQXIZg_Oy" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Challenges
 - *Cartesian Path Planner*: When initially incorporating the `find cartesian path` functionality using the `MoveIt` API, we encountered a challenge where `RViz` indicated that the robot had identified the path but was unable to execute it. To address this issue, we examined our code related to the `MoveIt` API, specifically focusing on the function responsible for calling the `ComputeCartesianPath` service. Upon comparing our implementation with the official `MoveIt` documentation, we identified a crucial missing parameter known as `cartesian_speed_limit_link`, which had not been specified in our code. Once we addressed this omission and provided the necessary parameter, the robot successfully executed the intended movements.
 - *TF tree when integrating `apriltags`*: Upon the initial implementation of `apriltags` on the robot, we encountered an issue where the robot occasionally failed to move as intended, leading to collisions when approaching certain orientations and positions. To address this challenge, our debugging process involved a thorough examination of the `TF tree` associated with the robot. We conducted numerous experiments by sending various commands, instructing the robot to move in all possible directions. During this investigation, a crucial insight emerged when analyzing the `TF tree`. It was discovered that with the introduction of `apriltags` into the system, the root frame of the `TF tree` shifted from `panda_link0`, the base frame of the robot, to `camera_link`. Consequently, the commands we were sending were relative to the `camera_link` frame rather than the base frame. Upon rectifying this discrepancy, specifically aligning the commands with the correct base frame, the robot executed movements flawlessly.

# Possible Improvements
 - Some script language still fail to detect: To resolve this issue, we can try to refind the language model by training more dataset on the script languages making it easier to detect the script languages.
 - Sometimes, the camera source get dropped, need to re-launch all: This happened because sometimes the `realsense` camera package does not detect the camera successfully so that it will throw can error when that happens. To address this issue, we can surround that with a protect function that catch the error when it throws and try it again.