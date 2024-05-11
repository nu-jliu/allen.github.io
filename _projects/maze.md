---
layout: project
title:  Maze Runner
categories: Python, PyGame, Wavefront, Recursive Backtracking, Randomized Prim
image: assets/images/maze.gif
# featured: false
# hidden: false
imagewidth: 50%
order: 23
---

<!-- Python, Wavefront, Recursive Backtracking, Randomized Prim -->

**Authors**: Allen Liu, Anuj Natray, Henry Brown, Ishani Narwankar, Leo Li

# Project Description
Lead the team in implementing the Wavefront and Recursive Backtracking solver to find the path from a start to goal within a randomly generated maze.

# Structure

## Wavefront Solver
1. Starting from the goal position, label its cell as 0
2. Repeat step 3-4:
3. At iteration `n`, for all celled newly labeled with `n`, find all adjacent cells that is empty, label that cell with `n+1`
4. If the one of the new cell is the start cell, exit the loop.
5. From the start cell, find the one adjacent cell that is labeled one less than the current one, go to that cell and repeat the process until reaches the goal. 
6. Output the path as the final solition.

## Recursive Backtracking
1. When robot starts always tried to go `East`, then `North`, `West` and `South`.
2. If one direction is not available (The spot is either a wall or visited), try another one.
3. If all directions are not available, go back to the previous step and try other directions.
4. Repeat the step 2-3 until it reaches the goal.
5. Output the final path.

# Amination

<iframe width="560" height="315" src="https://www.youtube.com/embed/QBnimOgBjeg?si=DBGshXhhqxGaSYix" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



# Challenges
 - *Solver algorithm*: When first implementing the algorithm for two solvers, it was difficult for us to understand the concept for the solver so we spent most of time drawing the process on a paper to visualize it. After we had a clear concept about what it was it was a smooth process for all of us.