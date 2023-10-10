---
title: Práctica 2 - Follow Line
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RM_img.jpg?raw=true"
description: Programing a follow line with PID by myself.
tags:
- Práctica 2 - Follow Line
- PID
- Post
- Robótica Móvil
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica Móvil" to program a follow line with a PID:

---

# **Follow Line**
This task presents us with the situation of a Formula 1 circuit with a red line in the middle of the road. The task is program the F1 car to follow line with a PID implemented.

## Planning the Implementation
The method that I have decided to implement is a reactive system with a PID controler.

While the car sense the image, get the line and the centoid then the car move but the move is controled by the PID which set the angular and linear velocity depending on the error sensed.

## Method Implementation
In this practice I will perform three subtasks, obtein and process the image and get the centroid, get the motion PIDs and perform the motion.

#### Image Process:
Firstly, I need to get the line I want to follow, so I will use opencv to get the image in RBG. After that I print a rectangle at the top of the image to keep only the values ​​close to the wheels of the car and thus obtain a better point to follow. Then I will convert the image from RGB to HSV because the filter wont be affected if the brightness or intensity of the color change.

After the implementation of the red filter I get the moments to get the centroid of the line. 

#### Motion Process
With the centroid of the line and the width of the image I can get a diference or error to use it to know where is the car in reference to the line. Knowing this I can move the car to follow the line.

To get the values for the motion I decided to used two PIDs controllers to implement the movement of the car. Whith those PID's the movement will be more smooth, it won't crash to the wall and it could go faster. I want to do two diferents PID, one to get the linear speed and to get the angular speed. 

#### Linear PID:  
In this PID I will modify the linear velocity.

The proportional controller is obtained by multiplying the error (the car position - the line centroid) by a constant "Kp". The constant "Kp" is found by testing which works better in my algorithm. If the speed is low the solution is better but if the speed increases the solution start to oscillate.

The integral controller is obteined by multiplying the acumulation of errors (the car position - the line centroid) by a constant "Ki". The constant "Ki" is found by testing which works better in my algorithm.

The derivative controller is obteined by multiplying the diferrence between the last error and the actual error (the car position - the line centroid) by a constant "Kd". The constant "Kd" is found by testing which works better in my algorithm. Also if "Kd" is to big, sometimes your turn velocity will increase so fast, and will make your car to turn and hit the wall.

Finaly the output value the PID return is the diference between the maximun linear velocity and the summation of the three controllers values goten.

Also if the value is over the maximun value or below the minimun value the PID controler return the minimun or maximun value.

#### Angular PID:  
In this PID I will modify the angular velocity.

The proportional controller is obtained by multiplying the error (the car position - the line centroid) by a constant "Kp". The constant "Kp" is found by testing which works better in my algorithm. If the speed is low the solution is better but if the speed increases the solution start to oscillate.

The integral controller is obteined by multiplying the acumulation of errors (the car position - the line centroid) by a constant "Ki". The constant "Ki" is found by testing which works better in my algorithm.

The derivative controller is obteined by multiplying the diferrence between the last error and the actual error (the car position - the line centroid) by a constant "Kd". The constant "Kd" is found by testing which works better in my algorithm. Also if "Kd" is to big, sometimes your turn velocity will increase so fast, and will make your car to turn and hit the wall.

Finaly the output value the PID return is the summation of the three controllers values goten.

Also if the value is over the maximun value or below the minimun value the PID controler return the minimun or maximun value.

## Used Libraries
The code libraries that I have used are numpy and opencv2: 
- The numpy library, is used to set the arrays. 
- I have used the opencv2 (cv2) library to obtain and process the image and extract the points of interest from the image and values ​​obtained by the car camera.


## Code Functions
For this task I needed to create two functions and a class with his init and two expecific functions for the PIDs. 

The first function, called "movement", is a function that is responsible to calculate the error, then obtaine the velocities form the PID and finaly set them.

- **Movement function:**
 
        def movement(cX):
            error = (378 - cX)
            if (error > 0):
                error = error / (378)
            else:
                error = error / (640 - 378)

            linear_vel = linear_pid.get_linear(error)
            angular_vel = angular_pid.get_angular(error)

            HAL.setV(linear_vel)
            HAL.setW(angular_vel)

The second function, called "image_processing", is a function that is responsible to get the image, process it to get the red line and its centroid.

- **Image_processing function:**
        def image_processing(cX):
            image = HAL.getImage() 
            heigth, width, channels = image.shape

            image = cv2.rectangle(image, (0, 0), (width, 350), (0,0,0), -1)
            
            hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
            
            img = cv2.inRange(hsv, np.array([0, 50, 50]), np.array([10, 255, 255]))

            M = cv2.moments(img)

            try:
                cX = int(M["m10"] / M["m00"])
            except ZeroDivisionError:
                cX = cX
            
            return img, cX

The PIDControler is a class defined to calculate the PID velocities more easily. This class id formed by the "_init_" function (constructor) and the functions "get_angular" and "get linear"

- **Init function:**

        def __init__(self, KP, KI, KD, min_ref, max_ref, min_v, max_v):
            # PIDControlers contructor
            self.KP_ = KP
            self.KI_ = KI
            self.KD_ = KD
            self.min_v_ = min_v
            self.max_v_ = max_v
            self.min_ref_ = min_ref
            self.max_ref_ = max_ref
            self.int_error_ = 0.0
            self.prev_error_ = 0.0

- **Anfular PID function:**

        def get_angular(self, error):
            # PID for angular speed
            error_ = error
            output = 0.0

            # Proportional Error
            direction = 0.0
            if error_ != 0.0:
                direction = error_ / abs(error_)

            if abs(error_) < self.min_ref_:
                output = 0.0
            elif abs(error_) > self.max_ref_:
                output = direction * self.max_v_
            else:
                output = (direction * self.min_v_ + error_ * (self.max_v_ - self.min_v_))

            # Integral Error
            self.int_error_ = (self.int_error_ + output) * 2.0 / 3.0

            # Derivative Error
            deriv_error = output - self.prev_error_
            self.prev_error_ = output

            output = (self.KP_ * output + self.KI_ * self.int_error_ + self.KD_ * deriv_error)
            
            # Limitar la salida a los valores máximos y mínimos
            return max(min(output, self.max_v_), -self.max_v_)

- **Linear PID function:**

        def get_linear(self, error):
            #PID for linear speed
            error_ = abs(error)
            output = 0.0

            # Proportional Error
            if abs(error_) < self.min_ref_:
                output = 0.0
            elif abs(error_) > self.max_ref_:
                output = self.max_v_
            else:
                output = (self.min_v_ + error_ * (self.max_v_ - self.min_v_))

            # Integral Error
            self.int_error_ = (self.int_error_ + output) * 2.0 / 3.0

            # Derivative Error
            deriv_error = output - self.prev_error_
            self.prev_error_ = output

            output = self.max_v_ - (self.KP_ * output + self.KI_ * self.int_error_ + self.KD_ * deriv_error)

            # Limitar la salida a los valores máximos y mínimos
            return max(min(output, self.max_v_), self.min_v_)


Also I have to mention that if you modify the PIDs constants and the linear and angular speeds the car will finish the circuit faster. I used this values due to they were the best after many attempts and simulations. 

## Gifts and videos

There are some gifts which shows the car following the line.  
**Gif of the camera filter:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1161353413284610149/img.gif?ex=6537fda6&is=652588a6&hm=714ff91de3395b82edf0059baf364f3afb3fa0e97245ffe5d1f6895b72ab4fd2&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p2_img.gif"></a></p>

**Gif of gazebo:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1161353413708238948/video.gif?ex=6537fda6&is=652588a6&hm=991236c3593ff1d12aefe726709dee177870db2a2a359a4b725c18619dc75067&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rm_p2_video.gif"></a></p>


This video shows three simulations of the follow line. 
- When the computer records the simulation slowing down the simulation.
- When the phone record the simulation with the car image output.
- When the phone record the simulation without the car image output been this the fastest simulation.

**Simulation Video**
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/HTfGv62qNIs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
 
