---
layout: project
title: Mobile Manipulator Control
description: Python, CoppeliaSim, Screw Theory
image: assets/images/mobile-manipulator-control/main.gif
imagewidth: 0
order: 997
---

[GitHub Repository](https://github.com/ngmor/robotic-manipulation-final)

While taking Northwestern University's ME449 Robotic Manipulation class, students complete a capstone project that combines several elements in the course together into a simulation controlling a mobile manipulator Kuka youBot.

The goal of this project is to use **modern screw theory** concepts along with the [**Modern Robotics**](https://github.com/NxRLab/ModernRobotics) library in **Python** to generate a trajectory for the youBot's end-effector to pick up a block and place it in a target location. **Feedforward + PI control** is used to eliminate error between the actual robot trajectory and the generated reference trajectory. The simulation is performed in CoppeliaSim, with ODE as an engine to simulate the physics of picking up the block in the youBot's gripper.

Here's a video of my solution to the problem:

{% include youtube.html video_id="Q9zL-nBLH3Q" width="75%" %}

<br>

{% details **<u>Table of Contents</u>** %}
- [Design](#design)
  - [Milestone 1: youBot Kinematics Simulator](#milestone-1-youbot-kinematics-simulator)
  - [Milestone 2: Reference Trajectory Generation](#milestone-2-reference-trajectory-generation)
  - [Milestone 3: Feedforward/Feedback Control](#milestone-3-feedforwardfeedback-control)
  - [Self-Collision Avoidance](#self-collision-avoidance)
- [Future Work](#future-work)
{% enddetails %}

****

## Design
The project is split up into several steps, called "milestones."

### Milestone 1: youBot Kinematics Simulator
The first step was to write code to simulate the youBot kinematics. The CoppeliaSim scene only performs physics simulations for the youBot gripper and the block, to determine whether the motion of the youBot succeeds to pick up the block. The actual motion of the youBot is input into the CoppeliaSim scene as a CSV which includes joint states, wheel positions, and chassis positions.

In order to create an accurate simulation (at least kinematically), I used first-order Euler integration to determine the new states of the joints and wheels based on the current state and input command velocities. The position of the chassis of the omnidirectional robot is dependent on the wheel motion, so I used odometry to then determine the change in chassis position from the wheel motion.

### Milestone 2: Reference Trajectory Generation
Next, a reference trajectory for the youBot end-effector is generated. This reference trajectory consists of 8 segments:

1. Initial end-effector position to a standoff position above the block
2. Pick standoff position down to pick position
3. Closing the gripper
4. Pick position up to pick standoff position
5. Pick standoff position to place standoff position above the block place location
6. Place standoff position down to place position
7. Opening the gripper
8. Place position up to place standoff position

These trajectory segments are generated as Cartesian (straight-line) or Screw (following a screw axis) trajectories based on the demands of each segment. Each point in the reference trajectory is defined by an SE(3) transformation matrix defining the position and orientation of the end-effector in the fixed world frame.

You can see the different trajectory segments in the below video (which also includes feedforward + feedback control to eliminate error in the beginning of the first segment).

{% include youtube.html video_id="0QBEBswZAlI" width="50%" %}
<br>

### Milestone 3: Feedforward/Feedback Control
The final aspect of the core project is feedforward/feedback control. In the simulation, the youBot may not start exactly at the desired starting position of the reference trajectory. To correct for this, feedforward and PI control is used to correct any error in the starting position by the end of the first trajectory segment, so the robot is successful in picking up the block.

The control law calculates an error twist based on actual position of the end-effector, the desired position of the end-effector, and the chosen feedback gains. This error twist is turned into command velocities using the full Jacobian of the youBot, which is constructed from the Jacobian of the manipulator arm and the characteristics of the omnidirectional wheelbase.

Finally, putting all the milestones together into a single workflow allows a CSV of joint/wheel/chassis states to be created, which defines the motion of the youBot in the simulation.


### Self-Collision Avoidance
Because dynamics and collisions of the youBot chassis/links are not being simulated in Milestone 1 or CoppeliaSim, with just the core project it is very possible for the youBot to collide with itself. In fact, without including collision avoidance, self-collisions were prone to happen. The method used to calculate command velocities uses the pseudoinverse of the youBot's Jacobian, which minimizes the motion needed to keep the end-effector on the target trajectory. Minimizing motion often coincides with movements that produce self-collision.

To handle this, I built in collision avoidance by including limits on the motion of the joints. To do so, feedforward/feedback control is calculated first, to get commanded velocities. Then the next timestep's joint positions are simulated. If any joints exceed their joint limits, that joint's column in the youBot's Jacobian is set to 0. This essentially mathematically states that motion of that joint will not affect the end-effector position, so a velocity of that joint is not commanded. After doing so, feedforward/feedback control is recalculated, and the process is repeated until no joint limit violations are detected.

This worked very effectively in preventing self-collisions. The previous videos on this page all have collision avoidance active, but as an illustration of its effect, here is a video without collision avoidance:

{% include youtube.html video_id="csmE6v4PY44" width="50%" %}

****

## Future Work
One area that could be improved in this solution would be trajectory timing. Right now, several time parameters (total time, gripper actuation time, standoff motion time) must be passed into the reference trajectory generation function in order for it to generate a reference trajectory. The behavior of the youBot can be very sensitive to these timing parameters. Generally longer times aren’t an issue, but if the total time for the simulation is too short, the youBot’s control algorithm will have to deal with very large errors due to high required speeds and will not work well.

One solution to this would be modifying the reference trajectory generation function to use the maximum speed of joints/wheels to determine a reasonable total time for the task and generate a reference trajectory accordingly. This would reduce the possibility of having the feedback/feedforward control malfunction due to large errors caused by high speed demands.