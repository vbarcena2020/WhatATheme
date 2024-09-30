---
title: Práctica 1 - Basic Vacuum Cleaner
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a basic vacuum cleaner by myself.
tags:
- Práctica 1 - Basic Vacuum Cleaner
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
Firstly, I made a three states machine but later I modify it and added another state, the going back state.

To do this task I have decided to divide it into two parts that match the sensors used, the bumper and the laser:  
#### Bumper:  
One of the sensors is the bumper. The bumper is a semicircular piece located on the front of the robot which sends signals if it is pressed. The bumper returns two values ​​mainly. The first tells you if it has been pressed or not and the second tells you where it has been pressed.
Using these two values ​​you can make a state machine that advances until the robot crashes and turns when it does so in the opposite direction to where it was hit.


#### Laser:  
The other sensor is the laser. The laser used in this practice is a 360 degree laser. But I am only going to focus on the first 180 degrees. Since the only values ​​that interest me are those found on the front because I am going to use them to detect if there is any object in front.


## Method Implemented
The method that I have decided to implement is a state machine with four states:
- State 1: This is the state that is responsible for making the robot move in a spiral and thus cover a lot of ground. But this only works on its own in large, circular environments with few or no obstacles. That is why two other states are needed.
- State 2: This state is responsible for acting in the event that the robot collides with or is approaching an object from the front. If any of these cases occur, the robot will stop advancing and will proceed to go back a little time.
- State 3: After going back the robot will start to rotate for a random time towards the opposite direction of the object.
- State 4: This is the last of the states and is the one responsible for making the robot advance in a straight line in order to move throughout the environment. This progresses for a while until two situations occur. The first is that it approaches or collides with a wall, which would cause it to change to state 2. The other option is that it randomly goes to state 1, producing a spiral sweep.

![State diagram](https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/drigrama%20de%20estados.png)

## Used Libraries
The code libraries that I have used are rospy and random: 
- The random library, as its name indicates, I have used to generate random numbers to produce the randomness of the pseudorandom algorithm. 
- I have used the rospy library to obtain the elapsed time and thus be able to control the spinning time in state 2 and state 3.


## Code Functions
For this task I only needed to create a function separate from the main function. This function, called laser, is a function that is responsible for verifying whether a wall or object has been detected within a specific range (in this case 0.3 meters). I have implemented this function in order to have a better structure of the code.


In this I have implemented a scan in two 45 degree fans with respect to the front angle of the laser. This allows me to detect which direction the object or wall is approaching.

- **Laser function:**
 
        def laser(laser_data):
            left_detected = False
            rigth_detected = False
            detected = False
            # Look at the front left
            for i in range(90, 135):
                if(laser_data.values[i] < 0.3):
                    left_detected = True
            # Look at the front rigth        
            for i in range(45, 90):
                if(laser_data.values[i] < 0.3):
                    rigth_detected = True
            # Change if something is detected
            if (rigth_detected or left_detected):
                detected = True
                
            return detected


## Code States
The three states are inside an infinite loop to perform the algorithm infinitely. At each iteration of the loop I call the functions to obtain both the status of the bumper, the laser data and whether the laser has detected. Afterwards it will move to one of the states. 

- **Function calls:**

        bumper_pressed = HAL.getBumperData().state
        laser_data = HAL.getLaserData()
        detected = laser(laser_data)

- **State1 code (Make the spiral):**
  
        if(state == 1 and bumper_pressed == 0 and detected == False):
            HAL.setV(v)
            HAL.setW(w)
            v += 0.0125


- **State2 code (Recover if it detected and object or it hit a wall going back):** 

        elif (HAL.getBumperData().state == 1 or detected == True or state == 2):
            if (first):
                state = 2   
                time1 = rospy.Time.now()
                first = False
            
            # Go back
            v = -1
            w = 0
            HAL.setV(v)
            HAL.setW(w)

            time2 = rospy.Time.now()

            # Wait some time randomly
            if (time2.secs - time1.secs > 0.5):
                state = 3
                first = True

- **State3 code (Recover if it detected and object or it hit a wall turning):** 

        elif (state == 3):
            if (first):
                state = 3 
                time1 = rospy.Time.now()
                first = False    
            v = 0
            w = 3
            HAL.setV(v)
            bump = HAL.getBumperData().bumper

            # Turn 
            if (bump == 0 or left_detected):
                HAL.setW(w)
            elif (bump != 0 or rigth_detected):
                HAL.setW(-w)

            time2 = rospy.Time.now()

            # Wait some time randomly
            if (time2.secs - time1.secs > random.uniform(0.5, 2.5)):
                state = 4
                first = True
  
- **State4 code (Go forward)** 

        elif (state == 4):
            v = 3.0
            w = 0.0
            HAL.setV(v)
            HAL.setW(w)
            ran = random.random()
            
            # Change to state 1 randomly       
            if (ran > 0.985):
                state = 1
                v = 0
                w = 3

## Critical Analysis

First of all, after trying so many times doing the spiral with diferents values of linear and angular velocities I get scenes where the same point was clean so many times and others where the vacuum left so many points without clean. If the linear speed increase slow the spiral will be more closed but if it increase fast the spiral will be more open and there will be points that wont be clean. I set the angular speed as a constant because it is easily to modify the increase of the linear speed to get the best spiral.

To avoid the wall or objets I set a going back and a turn funtions because it is easily to scape from corners. Also the values of time of the movement in both cases were tried to be the best in this scene with this robot. If the scene is diferent or the robot this times will have to be change.

Finally, the probability of change to state 4 to state 1 is tried to reach all the scene posible. Trying the code I notice that if the probability is higher the scene reached is lower because it enter in a continuous bucle of doing spirals. However, if the probability is lower the robot will go forward all the time and it wont enter in the spiral moving along the scene but leaving many spaces. But if the probability is in a logic range the robot will clean the house better and faster.

## Gifts and videos

Those are the gifts and videos of the two states machines.


**Gif of the three states:**

There is a gift which shows a test of the three states of the state machine. The spiral, the recovery if you find an object or hit a wall and going forward.  
<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1156626403991769098/p1_gif.gif?ex=6515a7c7&is=65145647&hm=065419dcd11567e5315de08ef080dc557b4b0453195c3ed5fd99bdb23cc45906&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/p1_gif.gif"></a></p>


**Gif of the four states:**

There is a gift which shows a test of the four states of the state machine. The spiral, the going back, the recovery if you find an object or hit a wall and going forward.  
<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1170463428813541416/gif_basic.gif?ex=65592203&is=6546ad03&hm=c7a05a5ef20b5497636b3d5344151e7578ec1654668941ec1cfefde89bc33d17&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p1.gif"></a></p>
**Simulation Video with three states**

This video shows two simulations of the pseudorandom algorithm with different probabilities of the spiral occurring. 
- When the probability is greater, the robot cleans an area several times but has a harder time moving around the entire house, although it does so over time.
- While when the probability is lower, it moves throughout the house more quickly but takes longer to clean an entire area perfectly.

<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/VDgjHM2GMqA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br>
**Simulation Video with four states**

This video shows two simulations of the pseudorandom algorithm with different probabilities of the spiral occurring. 
- When the probability is greater, the robot cleans an area several times but has a harder time moving around the entire house, although it does so over time.
- While when the probability is lower, it moves throughout the house more quickly but takes longer to clean an entire area perfectly.

<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/hfAQQ-whRiE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
