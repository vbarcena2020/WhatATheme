---
title: Práctica 3 - Autoparking
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RdS_img.jpg?raw=true"
description: Programing an autoparking car by myself.
tags:
- Práctica 3 - Autoparking
- post
- Robótica de Servicios
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica de Servicios" to program a autoparking car:

---
 
# **Autoparking**
This task presents us with two situations. In both situations, we want a car with three 180-degree lasers to be able to detect a parking space and perform the parking maneuver on its own. The difference between the two simulations is that in one the car is holonomic and in the other it is a more realistic car with Ackermann dynamics.

## Planning the Implementation
The method that I have decided to implement is a sequence system.

First, the car drives straight across the road until it detects with the laser located on its right a free parking space. Then proceed to make the movements of the parking maneuver using the rear and front laser of the vehicle to avoid crashing and know when to turn.

## Method Implementation
In this practice I will perform one subtask, detect with the laser.

#### Detect Laser Process:
To detect all the situations we need and the cars arround us I will use the lasers to know when I have to change the linear speed and the angular speed. 

## Used Libraries
The code libraries that I have used are time: 
- I have used time to prepare the car.

## Code Functions
For this task I needed to create two functions. 

The first function, called "detect_laser", is a function that is responsible to return a bool variable depending if the laser has sense something in the range of laser positions that have been passed.

- **Detect laser function:**
 
        def detect_laser(laser_data, detect, angle_min, angle_max, max_dist):

            laser = []
            for i in range (len(laser_data.values)):
                dist = laser_data.values[i]
                if i > angle_min and i < angle_max:
                    if (dist > max_dist):
                        dist = max_dist

                    laser += [(dist)]
                                
            if (all(valor >= max_dist for valor in laser)):
                detect = False
            else: 
                detect = True
                    
            return detect

The second function, called "center_in_park", is a function that is responsible to return a bool variable depending if the laser has sense that the car is center when there is no car in the front.

- **Center car in park function**

        def center_in_park(laser_data, first, state):
            if (first):
                first = False
                if(laser_data.values[80] > laser_data.values[100]):
                    state = 1
                elif(laser_data.values[80] < laser_data.values[100]):
                    state = 2
                return True, first, state
            else:
                if(state == 0):
                    return False, first, state
                elif(state == 1):
                    if(laser_data.values[80] < laser_data.values[100]):
                        return False, first, state
                elif(state == 2):
                    if(laser_data.values[80] > laser_data.values[100]):
                        return False, first, state
            
            return True, first, state
            
## Critical Analysis

Finally, I have to mention that to get the best solution to this task I will have to modify the values of the angular and linear speed and also the range of laser positions I will pass to the function "detect_laser" in all the cases to change to the next action.

It is also worth mentioning that I have tried to implement one more task to make the algorithm more robust but due to lack of time and not being able to achieve it, I have had to eliminate these two implementations.

The implementation was to try to make the car focus on the road with respect to the rest of the vehicles in case it was not already centered.


## Videos

**Simulation Video**

This video shows the simulations of the two cars, the holonomic and the Ackermann, doing the autoparking. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/WWRFu3M6DFY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
