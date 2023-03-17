---
layout: project
title: Legged Autonomous Inspection
description: C++, ROS 2, Nav2, RTAB-Map, OpenCV, Unitree Go1
image: assets/images/legged-autonomous-inspection/main.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/ngmor/unitree_inspection)

In my opinion, legged robots are some of the coolest robots out there! For my winter project in Northwestern University's MSR program, I was lucky enough to work with the quadruped [**Unitree Go1**](https://www.trossenrobotics.com/unitree-overview).

An interesting application of legged robots is autonomous inspection of industrial settings. Since they can traverse environments designed for humans better than their wheeled counterparts, legged robots are well suited to keeping tabs on an industrial environment. This can especially come in handy for environments that are inconvenient or hazardous for humans to visit.

My winter project focused on exploring this autonomous inspection application with the Unitree Go1.

{% include youtube.html video_id="onxPLXfmMvo" width="75%" %}

<br>

## The Application
In an industrial setting, an autonomous inspection might look something like the flowchart below. Legged robots can be provided with a map of their industrial setting with specific points of interest for inspection selected by plant engineers. Once the robot has reached each inspection point, it can use an array of sensors - visual, thermal, and more - to collect data. Potentially, if something about the collected data requires further investigation, the robot could decide to divert from its typical inspection route to a secondary inspection location and collect further data. It would continue this procedure until it had collected necessary data from all points of interest.

{:refdef: style="text-align: center;"}
![Industrial Inspection Flowchart](/assets/images/legged-autonomous-inspection/full-application-flowchart.svg){: width="60%"}
{: refdef}
{:refdef: style="text-align: center;"}
_A high level flow chart for how a legged robot could perform industrial inspections._
{: refdef}

I was particularly interested in exploring the autonomous decision-making aspect of this application. I wanted to create a robot that could sense some information, make a decision as to where to navigate next based on that information, and then autonomously navigate to the next point. I chose to present the data in the form of text located somewhere at the inspection point. The Go1 could use its cameras to locate and read the text, and then decide the next location to inspect based on the text content.

A real-world analogy to this capability would be visually reading data from display readouts throughout an industrial environment. Many display readouts, especially in older facilities, provide sensor data that cannot be networked to a plant SCADA system. Having the ability to detect text and extract data from those sensor readouts would be very versatile.

{:refdef: style="text-align: center;"}
![TODO](/assets/images/legged-autonomous-inspection/demo-flowchart.svg){: width="35%"}
{: refdef}
{:refdef: style="text-align: center;"}
_A high level flowchart for my legged autonomous inspection demo._
{: refdef}

## The System
Accomplishing this autonomous inspection task required several subsystems integrated together. These subsystems generally fell into two categories:
- **Navigation** (3D LiDAR, RTAB-Map, Nav2)
- **Optical Character Recognition** (Onboard Cameras, EAST Text Detection, CRNN Text Recognition)

with some control and high level logic packages to tie the subsystems together.

{:refdef: style="text-align: center;"}
![Block Diagram of the System Software Stack](/assets/images/legged-autonomous-inspection/system-diagram.svg){: width="60%" style="padding-right: 10%;"}
{: refdef}
{:refdef: style="text-align: center;"}
_A block diagram of the system software stack._
{: refdef}

### Navigation
A critical task for this project is autonomous navigation around a mapped environment and avoiding obstacles that may not have been present in the original map. Luckily ROS 2 has [**Nav2**](https://navigation.ros.org/), a software stack designed specifically for this task.

The first part of integrating Nav2 with the Unitree Go1 was sensing. The Unitree Go1 EDU Plus model has an option of coming with a [**RoboSense RS-Helios-16P 3D LiDAR**](https://www.robosense.ai/en/rslidar/RS-Helios), so I worked with [Marno Nel](https://marnonel6.github.io/) to set up this sensor for mapping of the environment. We chose to process the point cloud data provided by this sensor with [**RTAB-Map**](http://introlab.github.io/rtabmap/) for its 3D 6DoF mapping capabilities. RTAB-Map provides the Nav2 stack with [**ICP odometry**](https://en.wikipedia.org/wiki/Iterative_closest_point) and SLAM updates for the position of the robot and objects in the environment. Nav2 then uses this information to create local and global costmaps for planning paths that avoid collisions with obstacles in the environment.

The second part of integrating Nav2 with the Unitree Go1 was control. Nav2 can already plan paths and command velocities, but it needs some way to pass these commands through to the robot. I wrote a `cmd_processor` ROS 2 C++ node in the [`unitree_nav`](https://github.com/ngmor/unitree_nav) repository that takes in commanded velocities and translates them to control messages that can be interpreted by my communication node in the [`unitree_ros2`](https://github.com/katie-hughes/unitree_ros2) repository and sent to the Go1 over UDP. This node also handles other control capabilities, like commanding the Go1 to stand up and lay down. The other half of control was commanding Nav2 to plan and execute paths, which I wrote example code for in the `nav_to_pose` node of the `unitree_nav` repository.

Here's a video of the Nav2 stack working with the Unitree Go1 for autonomous navigation. The point cloud created by the 3D LiDAR is displayed in the top left and the created map in the bottom right.

{% include youtube.html video_id="oz92rP-ztPk" width="50%" %}

<br>

### Optical Character Recognition
{:refdef: style="text-align: center;"}
![TODO](/assets/images/legged-autonomous-inspection/woof-bark-arf-ruff.jpg){: width="35%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Reading dog words with the dog TODO._
{: refdef}

{% include youtube.html video_id="zDpLrAneXNg" width="50%" %}

<br>

## Future Work
TODO

Getting the dog off the leash
TODO - first ROS 2 Humble

Integrate IMU

Improve holonomic capabilities

Improving search



A big thanks to the other engineers who collaborated with me in getting the Go1 up and running. The autonomous inspection project was my own, but [**Marno Nel**](https://marnonel6.github.io/), [**Katie Hughes**](https://katie-hughes.github.io/), [**Ava Zahedi**](https://avazahedi.github.io/), and I worked together on upgrading and integrating the Go1 with ROS 2 Humble. Another post about that project is coming soon!