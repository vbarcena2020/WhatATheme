---
title: Práctica 1 - Basic Vacuum Cleaner
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a basic vacuum cleaner by myself.
tags:
- Basic Vacuum Cleaner
- Post
- Robótica Móvil
- Unibotics
- Python
- Docker
- Laser 
- Bumper
---

This post will show you how I progress in my task of subject "Robótica Móvil" to program a basic vacuum cleaner:

---

# **Basic Vacuum Cleaner**
## Planning the Implementation
To do this task I have decided to divide it into two parts that match the sensors used, the bumper and the laser:  
#### Bumper:  
One of the sensors is the bumper. The bumper is a semicircular piece located on the front of the robot which sends signals if it is pressed. The bumper returns two values ​​mainly. The first tells you if it has been pressed or not and the second tells you where it has been pressed.
Using these two values ​​you can make a state machine that advances until the robot crashes and turns when it does so in the opposite direction to where it was hit.


#### Laser:  
The other sensor is the laser. The laser used in this practice is a 360 degree laser. But I am only going to focus on the first 180 degrees. Since the only values ​​that interest me are those found on the front because I am going to use them to detect if there is any object in front.


## Method Implemented
The method that I have decided to implement is a state machine with only three states:
- State 1: This is the state that is responsible for making the robot move in a spiral and thus cover a lot of ground. But this only works on its own in large, circular environments with few or no obstacles. That is why two other states are needed.
- State 2: This state is responsible for acting in the event that the robot collides with or is approaching an object from the front. If any of these cases occur, the robot will stop advancing and will proceed to rotate for a random time towards the opposite direction of the object.
- State 3: This is the last of the states and is the one responsible for making the robot advance in a straight line in order to move throughout the environment. This progresses for a while until two situations occur. The first is that it approaches or collides with a wall, which would cause it to change to state 2. The other option is that it randomly goes to state 1, producing a spiral sweep.

## Code Functions
For this task I only needed to create a function separate from the main function. This function, called laser, is a function that is responsible for verifying whether a wall or object has been detected within a specific range (in this case 0.3 meters). I have implemented this function in order to have a better structure of the code.


In this I have implemented a scan in two 45 degree fans with respect to the front angle of the laser. This allows me to detect which direction the object or wall is approaching.

```py
def laser(laser_data):
    left_detected = False
    rigth_detected = False
    detected = False

    for i in range(90, 135):
        if(laser_data.values[i] < 0.3):
            left_detected = True

    for i in range(45, 90):
        if(laser_data.values[i] < 0.3):
            rigth_detected = True
    
    if (rigth_detected or left_detected):
        detected = True
        # print("detected")
        
    return detected
```
<!-- ![Code laser](https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/code_laser.png?raw=true) -->
<!-- 


**Giphy Gifs will look like:**<br>
<iframe src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/p1_gif.gif" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/p1_gif.gif">via GIPHY</a></p> -->
<!-- 
**YouTUbe Videos will look like:**<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/jTPXwbDtIpA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  -->