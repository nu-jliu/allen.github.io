---
layout: project
title:  Rapidly-Exploring Random Tree
description: Python, RRT
image: assets/images/rrt_nu.gif
# featured: false
# hidden: false
imagewidth: 50%
order: 22
---

<!-- Python, RRT -->

**Authors**: Allen Liu

**GitHub**: [View This Project on GitHub](https://github.com/nu-jliu/hackathon_RRT)

# Project Description
Implementing the `Rapidly-Exploring Random Tree` (`RRT`) algorithm in finding a path from the start to goal while avoiding all obstacles.

# Architesture
## Tree Structure
The tree is structured as shown below:
 - A tree is consist of edge and verticies.
 - Each vertex has only one parent as many children.
 - The root vertex does not have a parent.
 - Tree class contains the list of vertices and the root vertice
 - The vertice class contains the parent vertice and the location of the vertice

## Algorithm
The RRT algorithm can be represented as following steps:
1. Random choose a point in the map called `q_r`
2. For all vert, connect it with `q_r`, make a segment with length 1 in that direction.
3. For all segments, if there is one segment that is neigher colliding with all obstacles shown in the map nor intersect with any of the existing edges, draw that segment on the gragh, and save its tail as a new vertice.
4. If no one segment satisified with the condition, regenerate a point `q_r` and redo all the processes shown above again.

# Amination

## Planing Path on Map with Oval Obstacles

<iframe width="560" height="315" src="https://www.youtube.com/embed/ehhinQ4TM8k?si=cIay6DGo2s0U59jP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


## Planning Path on Map with Northwestern Logo

<iframe width="560" height="315" src="https://www.youtube.com/embed/4pJUayLRvhQ?si=pIaZisnh-ZUt4bFK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


# Challenges
 - *Obstacle Avoidance*: In this project, I successfully tackled the primary challenge of determining the optimal line segment from a given vertex to a randomly generated point while ensuring avoidance of collisions with obstacles and existing edges. To overcome this obstacle, I leveraged vector calculus to calculate the distance between the line segment and obstacles. Additionally, I adopted a modeling approach, treating all existing vertices as obstacles with a fixed radius of 1. This strategic modeling guarantees the non-collision of newly generated vertices with their existing counterparts, contributing to the overall success of the project.

# Possible improvements.
 - To address the program's suboptimal performance, I plan to implement multi-threading in the future. This approach aims to enhance computational efficiency by enabling simultaneous execution of multiple tasks, ultimately reducing the overall processing time for the entire algorithm. The strategic adoption of multi-threading is anticipated to significantly boost the program's performance, contributing to improved responsiveness and user experience.