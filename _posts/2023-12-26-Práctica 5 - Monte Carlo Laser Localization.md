---
title: Práctica 5 - Monte Carlo Laser Localization
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a Monte Carlo localitation using a laser by myself.
tags:
- Práctica 5 - Monte Carlo Laser Localization
- Monte Carlo Localization
- Lidar
- Post
- Robótica Móvil
- No Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica Móvil" to program the algorithm Monte Carlo to localizate using a laser:

---

# **Monte Carlo Laser Localization**
This task presents us with the situation of a robot in a room only with a laser. We have to localizate using the map and the lidar the robot have.

The environment used on this occasion is not unibotics but a package with python functions that will simulate both the robot and the rest of the necessary applications to develop an algorithm that allows us to locate the robot with a map and a laser. You can find the package with the code in this [link](https://1drv.ms/u/s!AvvzEJ5P-1ZEix7hiiUddtGtqX7E?e=IonS1M  )

## Planning the Implementation
The method that I have decided to implement is the algorithm of Monte Carlo.

Firstly, I create a lot of particles in room and then I modify the weights of each particle depending on the measurements that the laser senses and the measurements that each particle should sense in the position of the map in which it is located. With this I can approximate the location of the robot, only if the particles describe the same movement as the robot.

## Method Implementation
Regarding the implementation of the code, most of it was understanding the functions and the code provided by our teacher in order to know how to use it to achieve our algorithm.

The method is a reactive system that is responsible for simulating the laser of the particles and establishing weights for each particle to regroup them until they focus on a group that must be the position of the robot.

Most of the code, as I have already mentioned, is in the one provided by the teacher, except for some small modifications. However, the interesting part of the developed algorithm focuses on how to establish the weights of the particles.

### Weights
To establish the weights of each particle I have decided to take into account several factors such as if the particle is on a wall or outside the size of the image it will instantly have a weight of 0 in order to be instantly regrouped. This is because these are impossible situations for the robot and since what interests us is locating the robot we have to imagine that each particle is a real robot.

Also to assign weights I take into account that I would see the robot's laser at the position of each particle, therefore I create an array with the simulated laser values ​​at that position.
With these values ​​and those of the robot laser I obtain the difference and with the maximum and minimum difference I establish the weights for each particle.

## Critical Analysis

Finally, I have to mention that depending mostly on the number of particles and the number of lasers in which they are caught per particle the simulation of the algorithm will take longer for each iteration due to the large number of calculations. 

At the same time, if there are not enough particles, the robot will never be able to locate it, or almost never. In my case, the number of particles that I have decided to use and that in most cases I get a fairly decent result is with 500 particles, but with this amount it is a little slow in terms of iteration. Also the simulation with 200 particles get a fairly decent result quite fluidly.

## Gifts and videos

There is a gift which shows the gradient path planning.

**Gif of the obstacle avoidance:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1189260696349843587/gif_Hecho_con_Clipchamp.gif?ex=659d8455&is=658b0f55&hm=6daf1cf185e61d0454e2d246cabeedba3f8fe808486289d6ff9439c473f69e00&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p5.gif"></a></p>

**Simulation Video**

This video shows the simulation of the global navigation with gradient path planning. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/18LbklhHvhI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
