---
layout: project
title:  Fixing Rethink Sawyer Robot Arm
Description: CMOS, Rethink Sawyer Robot Arm, Intra OS, Embedded Linux, Robot Hardware
image: assets/images/sawyer.jpg
# featured: false
# hidden: false
imagewidth: 50%
order: 20
---

CMOS, Rethink Sawyer Arm, Intera OS, Embedded Linux, Robot Hardware

# Problem Description

![](/assets/images/sawyer_detail.jpg){: style="width: 50%"}

For the winter project, I am using a mobile manipulator called `sawback`, which is a `Rethink Sawyer` robot arm mounted on the `Clearoath Ridgeback` mobile platform. Before I am able to use the robot arm for my project, I experience tons of challenged trying to setting it up for my robot arm.

## Booting Up

At the beginning, the robot was not able to boot up since the CMOS battery died, that system clock stoped. After changing the CMOS battery, I encountered another Issue that I could not send the command to for it to move as intended.

## Enabling the Robot

The robot has two mode one is SDK mode and another one is the Intera mode where SDK mode allowes you to control the robot with the intera SDK over `ROS` while the Intra mode allows people to control the arm using the pre-programmed controlling interface via buttons on the robot arm itself. After booting up, is was supposed to enabling the robot by pressing knob on the arm. It showed error that
```
[ERROR] Failed to enable robot, press the knob to try again.
```
while flashing the red light indicating the joint `right_j1` is not able to unclocked. I noticed that the issue comes from the unclocking mechanism on joint 1 of the `sawyer` arm.

## Fixing the Joint

Since it is clear that there is a issue with the unclocking mechanism for joint 1. In order to fix it, it is required to open it up to discover any mechanical/electrical issue within it.

### Encoder
First, I suspected that there is some issue within the encoder so I opened it up and tried to check whether there is a losen wire connection within the wire. I followed each wire, and used a digial multimeter to check the discontinuity with any connection. It turns out that there was no missed connection.

### Locking Mehcanism
After checking the encoder, I opened the joint trying to analyze the potential issue with it. Then I discovered that there is a solenoid attached to the joint serving as the locking mechanism for the joint that when it has not been powered, it will lock the gear such that prevent the joint from moving.

#### Solenoid
After inspecting with the solenoid, I discovered the issue that the solenoid would not be able to unlock the joint when I push it, setting it into the `unlock` mode.

#### Spring Detached
After I open it up, I discovered the issue that its spring has been detached its plate. The solenoid was designed to have a plate that the spring acts on so that it will push it into the unlock state when solenoid is activated. So I re-attach the plate and replace the spring into where is should be. Then the joint can be activated when I powered it on.