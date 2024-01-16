---
title: Práctica 4 - Amazon Warehouse
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RdS_img.jpg?raw=true"
description: Programing an amazon warehouse robot by myself using OMPL to get the path.
tags:
- Práctica 4 - Amazon Warehouse
- post
- Robótica de Servicios
- OMPL
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica de Servicios" to program an amazon warehouse robot:

---
 
# **Amazon Warehouse**
This task presents us with the situation of a robot in an Amazon warehouse which can lift and put down shelves. The robot has to use OMPL to get the path to the known points where the shelves are located, reach these points following the path and lift the shelves and bring them to the central point where it was located.

## Planning the Implementation
The method that I have decided to implement is a sequence system with diferents states.

First, the robot get the path, then it follows it and arrive to the shelve. Then it turn and lift the shelve and follow the path back to get to the origin position and put down the shelve in there after stoping

## Method Implementation
In this practice I will perform substask to convert to gaxebo coordenates to pixels and the absolute2relative function used before, also I used the OMPL class.

## Used Libraries
The code libraries that I have used are time: 
- I have used numpy.
- I have used math.
- I have use opencv to get the path and also os.path and sys libraries needed by OMPL.


## Code Functions
For this task I needed to use two functions. 

The first function, called "absolute2relative".

- **Absolute2relative function:**
 
        def absolute2relative (x_abs, y_abs, robotx, roboty, robott):
            dx = x_abs - robotx
            dy = y_abs - roboty

            x_rel = dx * math.cos (-robott) - dy * math.sin (-robott)
            y_rel = dx * math.sin (-robott) + dy * math.cos (-robott)
            
            return [x_rel, y_rel]

The second function, called "convert_to_pixels", is a function that is responsible to return in the pixels the corresponding coordinates in the real world.

- **Convert to pixels function**

        def convert_to_pixels(x_real_world, y_real_world):
            proporcion_x = (-x_real_world + high_real_world / 2) / high_real_world
            proporcion_y = (-y_real_world + width_real_world / 2) / width_real_world
        
            x_pixels = proporcion_x * high_pixels
            y_pixels = proporcion_y * width_pixels

            return int(x_pixels), int(y_pixels)
            
## Critical Analysis

Finally, I have to mention that to get the path I used the 2D point planning using a PPM image as a map in which an image is used as a map and defines the robot as a point, also I used opencv instead of ppm so I have to convert the code to opencv, the original code with ppm is in this [link](https://ompl.kavrakilab.org/Point2DPlanning_8py_source.html).

Also I have to mention that to follow the path I used a lower velocities because if the velocities are high enough, this will pass the point and to recover it it must make a complete turn causing it to collide with the surrounding objects and be lost.


## Videos

**Simulation Video**

This video shows the simulation of the robot going to the first shelve and move it to the origin point. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/vaddYR3I3ww" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
