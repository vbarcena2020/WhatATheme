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
![Code laser](https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/code_laser.png?raw=true)

**Normal text in the post will look like**<br>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris id finibus nisl. Etiam in hendrerit est. Nulla non erat ac lectus interdum lobortis. Vestibulum at mi ex. Mauris nisl mi, venenatis et feugiat nec, finibus porttitor velit. Suspendisse tincidunt lobortis leo, quis tristique tellus iaculis quis. Donec eleifend pulvinar gravida. Proin non lorem eros. Donec sit amet finibus ex, eget vestibulum nunc. Ut ut enim id purus porttitor tristique. Vivamus tincidunt eleifend hendrerit. Proin metus felis, ultrices vel dui in, porta dapibus dui. Sed sagittis ex vitae dui tristique dignissim. Cras vel leo ipsum.

Aenean ac neque et risus mattis accumsan. Sed ac tellus molestie, lacinia ante sit amet, convallis felis. Maecenas aliquet lectus nec euismod auctor. Donec finibus pellentesque tortor, ac efficitur metus suscipit non. Proin diam orci, blandit quis malesuada ac, efficitur a nisl. Mauris eleifend consequat blandit. Sed egestas quam et orci gravida, non euismod metus scelerisque. Curabitur venenatis pellentesque erat commodo pharetra. Fusce id ante nec ipsum fringilla auctor. In justo quam, feugiat placerat eleifend dapibus, luctus et quam. Fusce facilisis erat ut odio convallis viverra et id mauris. Sed vehicula tempus consectetur. Aliquam pharetra, purus non egestas tristique, tellus massa fringilla est, id sagittis tellus urna non mauris. Suspendisse fringilla, velit nec blandit facilisis, ligula ante imperdiet est, et placerat magna sem quis tortor.

Vestibulum vitae fermentum velit, rhoncus egestas orci. Nulla at purus ut orci posuere vulputate. In eget leo diam. In congue in diam nec elementum. Suspendisse fringilla ante nulla, eu tristique orci ultrices eget. Aenean non lorem tellus. Vestibulum tempor metus sit amet tellus feugiat, sit amet consequat lacus ultricies.

Donec imperdiet, lectus eget congue cursus, dolor enim finibus risus, ut molestie lorem tellus non tortor. Donec quam nibh, molestie in dapibus et, efficitur non tortor. Morbi orci tellus, mollis vel mi vitae, auctor lobortis erat. Ut gravida velit eget ligula lacinia, id rhoncus tellus gravida. Maecenas laoreet rutrum consequat. Suspendisse sed nibh dui. Curabitur dictum euismod mollis. Sed egestas libero libero, eu accumsan augue placerat non. Nunc id condimentum orci. Mauris vitae sollicitudin quam.

**Giphy Gifs will look like:**<br>
<iframe src="https://giphy.com/embed/ZqlvCTNHpqrio" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/laughing-despicable-me-minions-ZqlvCTNHpqrio">via GIPHY</a></p>

**YouTUbe Videos will look like:**<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/jTPXwbDtIpA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 