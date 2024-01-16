---
title: Práctica 1 - Located Vacuum Cleaner
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RdS_img.jpg?raw=true"
description: Programing a located vacuum cleaner by myself.
tags:
- Práctica 1 - Located Vacuum Cleaner
- post
- Robótica de Servicios
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica de Servicios" to program a located vacuum cleaner:

---

# **Located Vacuum Cleaner**
This task presents us with the situation the situation of a vacuum cleaner which must clean the house by locating it using the map.

## Planning the Implementation
The method that I have decided to implement is get the path using opencv and with the path move along the house to clean it.

I get the path using BSA in python but not in unibotics because I wanted to watch the where is the path drawing it in the image.

## Method Implementation
In this practice I will perform two subtasks, get the path, follow the path.

#### Get the Path
Fistly, I get the image with opencv and I resized it to 512 x 512 pixels and I erode the walls to avoid being to close to crash with those. 

After knowing the size of the image I divide it in cells of 16 x 16 pixels. 

To draw the path I get the origin point of the robot and draw the cell where it is in blue. The cells that have already been visited are drawn in blue and the cells of neighbors that have not yet been visited are drawn in green.

To move along the house I establish the priority of the following order west, north, east and south. When the robot is surrounded by previously visited places or walls I look for the closest and most accessible neighbor to go to and continue cleaning the house.

#### Follow the Path

To follow the path I use a reduced one in which all the consecutive repeated movements are put together, obtaining a path with fewer steps.

To move I convert the pixels to coordinates in the real world and with this coordinates I can go to then and orientate to the next position and follow all the path.

## Used Libraries
The code libraries that I have used are numpy, math and queue: 
- The numpy library, is used to set the arrays. 
- I have used math to calculate all the values needed.
- Opencv library, to process the image.


## Critical Analysis

Finally, I have to mention that depending on the values of the velocities the robot can lost the point and crash to the walls. 

Also you can modify sizes of the cells or not erode the walls but you risk getting too close to the walls and crashing, even if you get a better sweep of the house, in addition to adding many more steps to the path.

Finally, you can also modify the order of priority you give to the robot's cardinal points, obtaining a different path which can be better or worse, depending on where you start.


## Gifts and videos

There is a gift which shows the path creating.

**Gif:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1196949070334402592/path.gif?ex=65b97cb0&is=65a707b0&hm=cb43d0b2d7311cf76c59952caa7b7f6db6f185a3d7744a61ca89312ac493b74f&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rs_p1.gif"></a></p>

**Simulation Video**

This video shows the simulation of create the path and the localized vacuum cleaner in unibotics. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/3vREMZ11yZE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>