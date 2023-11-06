---
title: Práctica 3 - Obstacle Avoidance
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a follow line with PID by myself.
tags:
- Práctica 3 - Obstacle Avoidance
- Lidar
- Post
- Robótica Móvil
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica Móvil" to program a obstacle avoidance detected by a laser:

---

# **Obstacle Avoidance**
This task presents us with the situation of a Formula 1 circuit with some cars in diferent positions. The task is program the F1 car to go to the targets avoiding the cars in the circuit and the walls.

## Planning the Implementation
The method that I have decided to implement is a reactive system.

While the car sense the lidar, calculate the atractive force to the target, calculate the repulsive force of the close objects and move by the resultant force.

## Method Implementation
In this practice I will perform three subtasks, calculate the atractive force, calculate the repulsive force by the lidar and calculate the resultant force.

#### Atractive Force Process:
Firstly, I need to get the target x and y positions and the robot x, y and yaw position. With those values I call the "get_attractive_force()" function wich calculate the vector of atractive force to the target.

Then for greater ease when processing data I normalize the vector. 

#### Repulsive Force Process:
To calculate the repulsive force firstly I need to get the laser data and parse this data. To parse the data I decided to get only the distances of the sides of the car lower than a constant and the distances of the front of the car lower than an other constant. With the distances and angles values I can calculate the x and y distances and then get a mean of those values and erased some lower errors. 

Then for greater ease when processing data I normalize the vector. 

#### Resultant Force Process:  
The resultant force that I get is formed by the repulsive and atractive forces multiplied by constants.

The formula that I used is: **F_resultant = Const_A * F_atractive + Const_R * F_Repulsive**

## Used Libraries
The code libraries that I have used are numpy and math: 
- The numpy library, is used to set the arrays. 
- I have used math to calculate all the values needed.

## Code Functions
For this task I needed to create five functions. 

The first function, called "get_attractive_force", is a function that is responsible to calculate the attractive force by the robot and the target positions.

- **Get_attractive_force function:**
 
        def get_attractive_force(x_abs, y_abs, robotx, roboty, robott):
            dx = x_abs - robotx
            dy = y_abs - roboty

            attractive_x = dx * math.cos (-robott) - dy * math.sin (-robott)
            attractive_y = dx * math.sin (-robott) + dy * math.cos (-robott)

            return [attractive_x, attractive_y]

The second function, called "get_repulsive_force", is a function that is responsible to calculate attractive force by the parsed laser data.

- **Get_repulsive_force function:**
        
        def get_repulsive_force(laser):
            laser_vector = []
            for dist, angle in laser:
                x = -dist * math.cos(angle) 
                y = -dist * math.sin(angle) 
                laser_vector += [(x, y)]

            repulsive = np.mean(laser_vector, axis=0)
            
            if abs(repulsive[0]) < 0.01:
                repulsive[0] = 0

            if abs(repulsive[1]) < 0.01:
                repulsive[1] = 0

            return repulsive

The third function, called "parse_laser_data", is a function that is responsible to get only the usefull values of the laser.

- **Parse_laser_data function:**

        def parse_laser_data(laser_data):
            laser = []

            for i in range (180):
                if i < 60 or i > 120:
                    dist = laser_data.values[i]
                    if dist > Const_max_sides:
                        dist = 0
                else:
                    dist = laser_data.values[i]
                    if dist > Const_max_front:
                        dist = 0
                angle = math.radians(i-90)
                laser += [(dist, angle)]

            return laser


## Critical Analysis

Finally, I have to mention that to get the best resultant force I need to modify the four constants in order to give the best priority to the forces and ensure that the formula 1 does not collide with the walls or other vehicles because the force of attraction exceeds the force of repulsion or that it cannot reach the objective and stops or begins to spin around. that the force of repulsion is greater than the force of attraction. 

Also I have established these distances in the laser for the front and side of the Formula 1 since they are the ones that the car does best to avoid obstacles. If the values ​​are very small the car will crash and if they are too large it will not be able to pass between obstacles and walls.

## Images, Gifts and videos

There are some gifts which shows the car avoiding the obstacles.

**Image of the car forces:**<br>  

![car forces](https://cdn.discordapp.com/attachments/828395914145431612/1170457770999419010/foto_avoidance.png?ex=65591cbe&is=6546a7be&hm=ce40d71a244fcc83a5ce6e66b45fa8c63ee6256d3e4954b93302902f6a6e33b9&)


**Gif of the obstacle avoidance:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1170457770613547069/gif_avoidance.gif?ex=65591cbe&is=6546a7be&hm=997b065d3a87b07b1e39fca23829798345ff6f0f0144328fe7f2c5afd5b00738&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p3.gif"></a></p>

**Simulation Video**

This video shows the simulations of the obstacle avoidance of the car while going around the circuit. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/_WWscjTYS5E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
