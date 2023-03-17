---
layout: project
title: Legged Autonomous Inspection
description: C++, ROS 2, Nav2, Machine Learning, Unitree Go1
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

****

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

****

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

****

### Navigation
A critical task for this project is autonomous navigation around a mapped environment and avoiding obstacles that may not have been present in the original map. Luckily ROS 2 has [**Nav2**](https://navigation.ros.org/), a software stack designed specifically for this task.

The first part of integrating Nav2 with the Unitree Go1 was sensing. The Unitree Go1 EDU Plus model has an option of coming with a [**RoboSense RS-Helios-16P 3D LiDAR**](https://www.robosense.ai/en/rslidar/RS-Helios), so I worked with [Marno Nel](https://marnonel6.github.io/) to set up this sensor for mapping of the environment. We chose to process the point cloud data provided by this sensor with [**RTAB-Map**](http://introlab.github.io/rtabmap/) for its 3D 6DoF mapping capabilities. RTAB-Map provides the Nav2 stack with [**ICP odometry**](https://en.wikipedia.org/wiki/Iterative_closest_point) and SLAM updates for the position of the robot and objects in the environment. Nav2 then uses this information to create local and global costmaps for planning paths that avoid collisions with obstacles in the environment.

The second part of integrating Nav2 with the Unitree Go1 was control. Nav2 can already plan paths and command velocities, but it needs some way to pass these commands through to the robot. I wrote a `cmd_processor` ROS 2 C++ node in the [`unitree_nav`](https://github.com/ngmor/unitree_nav) repository that takes in commanded velocities and translates them to control messages that can be interpreted by my communication node in the [`unitree_ros2`](https://github.com/katie-hughes/unitree_ros2) repository and sent to the Go1 over UDP. This node also handles other control capabilities, like commanding the Go1 to stand up and lay down. The other half of control was commanding Nav2 to plan and execute paths, which I wrote example code for in the `nav_to_pose` node of the `unitree_nav` repository.

Here's a video of the Nav2 stack working with the Unitree Go1 for autonomous navigation. The point cloud created by the 3D LiDAR is displayed in the top left and the created map in the bottom right.

{% include youtube.html video_id="oz92rP-ztPk" width="50%" %}

<br>

****

### Optical Character Recognition
The second required subsystem is the visual text detection and recognition, referred to here as Optical Character Recognition (OCR). This project uses the Go1's onboard stereo cameras and two pre-trained machine learning models to accomplish this task.

In order to get image data from the Go1's onboard cameras, I wrote a [ROS 2 C++ wrapper](https://github.com/ngmor/unitree_camera) for the [**Unitree Camera SDK**](https://github.com/ngmor/UnitreecameraSDK) that uses [`image transport`](https://github.com/ros-perception/image_common/tree/ros2/image_transport) for image compression. This publishes raw, rectified, depth, and point cloud images from any of the Go1's five onboard cameras.

TODO camera media here

The first machine learning model uses a nueral network based on a [TensorFlow re-implementation](https://github.com/argman/EAST) of the [**Efficient and Accurate Scene Text Detector (EAST)**](https://arxiv.org/abs/1704.03155v2) model. This pipeline is designed to detect where text is located in a natural scene so that a text recognition model can parse it into characters. The EAST model provides bounding vertices for lines of text in arbitrary orientations, as shown by the green bounding rectangles in the image below.

{:refdef: style="text-align: center;"}
![TODO](/assets/images/legged-autonomous-inspection/woof-bark-arf-ruff.jpg){: width="35%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Making the dog read dog words._
{: refdef}

The second machine learning model uses a **convolutional recurrent nueral network (CRNN)** trained on the [MJSynth](https://www.robots.ox.ac.uk/~vgg/data/text/) and [SynthText](https://www.robots.ox.ac.uk/~vgg/data/scenetext/) datasets. It accepts the bounding vertices from the EAST model and parses the image cropped at those vertices into actual text, shown in red above.

The final step in inspecting for text is controlling the Go1 to do so. I wrote a simple pitch sweeping sequence that commands the Go1 to sweep its head front camera up and down until it reliably detects text that it recognizes as a command for a next inspection point. This is demonstrated in the video below.

{% include youtube.html video_id="zDpLrAneXNg" width="50%" %}

<br>

****

## Future Work
Putting the navigation and OCR subsystems together creates a system that can pretty reliably navigate between inspection points in an arbitrary order and avoid obstacles along the way. But as with any project, there is always improvements that can be made.

**1. Getting the dog off the leash.**

As can be seen in the demo video, an ethernet cable connects my laptop to the Go1's network of internal computers. This is partially so I can run visualization of the map and point cloud data in RVIZ, but it's also because our Go1's onboard computers are not yet capable of running all the required nodes for this project.

One of the most challenging parts of this project (on which a separate post is coming soon!) was upgrading the Go1's onboard NVIDIA Jetson Nanos to Ubuntu 22.04. The Nanos come with Ubuntu 18.04 - two LTS's behind the only Tier 1 supported Linux OS for ROS 2 Humble (Ubuntu 22.04). [Katie Hughes](https://katie-hughes.github.io/) and I worked together with help from this [awesome blog from Q-engineering](https://qengineering.eu/install-ubuntu-20.04-on-jetson-nano.html) to upgrade the Go1 to what we believe to be the only ROS 2 Humble version out there at the time of writing this post.

Unfortunately, we did not have time to also upgrade the Jetson Xavier NX on the Go1, which has more computing power. Getting the Xavier on 22.04 would allow us to run point cloud processing nodes at reasonable speeds on the Go1. We also currently have some limited wireless control of the Go1, which could be improved by bridging the onboard network through the Go1 Raspberry Pi's WiFi adapter. These two improvements would allow us to let the dog off the leash and create a truly mobile autonomous inspection bot.

**2. Integrating IMU odometry.**

The Go1 provides some odometry calculated from data from its onboard IMU, with limited accuracy. [Marno Nel](https://marnonel6.github.io/) and I experimented with integrating this with the Nav2 stack for faster odometry updates, but we weren't able to get to a comparison with the ICP odometry. Onboard IMU odometry may help improve localization by providing faster odometry updates in between the LiDAR-based SLAM updates. Our progress can be found in the [`onboard-odometry`](https://github.com/ngmor/unitree_nav/tree/onboard-odometry) branch of the `unitree_nav` repo.

**3. Improving holonomic capabilities.**

The Go1 can move forward, backward, and to each side. At current, Nav2 only ever plans paths where the robot moves in the forward and backward directions and rotates. With a little more investigation, this could be improved so the robot can move side to side too when planning paths!

**4. Improving data search capabilities.**

Currently, when searching for data, the Go1 only sweeps up and down from its current position. If there was some error in the navigation, this can sometimes cause it to miss the data at the inspection point. Improved algorithms to move the Go1 left and right slightly might help find the data more reliably.

**5. Adding other sensors.**

Visual inspection is great and could be extended to further cases. But it would also be versatile to include other sensors such as IR cameras to improve the dog's sensing capabilities.


****

A big thanks to the other engineers who collaborated with me in getting the Go1 up and running. The autonomous inspection project was my own, but [**Marno Nel**](https://marnonel6.github.io/), [**Katie Hughes**](https://katie-hughes.github.io/), [**Ava Zahedi**](https://avazahedi.github.io/), and I worked together on upgrading and integrating the Go1 with ROS 2 Humble. Another post about that project is coming soon!