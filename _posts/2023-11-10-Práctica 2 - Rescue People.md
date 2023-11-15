---
title: Práctica 2 - Rescue People
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/RdS_img.jpg?raw=true"
description: Programing a recue people with dron by myself.
tags:
- Práctica 2 - Rescue People
- post
- Robótica de Servicios
- Unibotics
- Python
- Docker
---

This post will show you how I progress in my task of subject "Robótica de Servicios" to program a dron who will rescue some people in the sea:

---

# **Obstacle Avoidance**
This task presents us with the situation of a missing people in the middle of the sea and a dron in a boat prepared to look for them. Also we know a near GPS coordiantes where the missing people where.

## Planning the Implementation
The method that I have decided to implement is a mix of a sequence and reactive system.

Fist, the dron will take of, then it will go to the known coordinates. After this it will look for faces while it do an spiral. After some minutes it will go back to the boat and land on it. 

## Method Implementation
In this practice I will perform two subtasks, detect people and do the spiral.

#### Detect People Process:
To detect people I will use Haar a tool of opencv which will return a point of the people sensed. With this points I will get the number of people it get and the positions where they are. The image I will send to haar is a zoomed image of the ventral image of the dron. I will use the zoomed image because I will only recognise the people centered under the dron to avoid repeated faces and people.

Also I implemented a method to avoid repeating faces. This method compare the established positions of the faces sensed and the new faces It sensed and if the positions are not the same it is because the face is a new face so I will save this new position.


#### Spiral Process:
This process will make an spiral to search for peaple in the nears of the GPS coordinates known. To do this spiral I will modify the linear speed in x while I keep the rotation of the dron and the altitude consts.

## Used Libraries
The code libraries that I have used are numpy and math: 
- I have used opencv to detect people with Haar.

## Code Functions
For this task I needed to create three functions. 

The first function, called "zoom_image", is a function that is responsible to get a zoomed image of the original and return this image. You can change the zoom_factor to get less points or more as you want. 

- **Zoomed image function:**
 
        def zoom_image(img):
    
            height, width, _ = img.shape
            zoom_factor = 2
            
            new_width = int(width / zoom_factor)
            new_height = int(height / zoom_factor)
            
            x = int((width - new_width) / 2)
            y = int((height - new_height) / 2)
            
            new_img = img[y:y+new_height, x:x+new_width]
            
            zoomed_img = cv2.resize(new_img, (width, height))
            
            GUI.showImage(zoomed_img)
            
            return zoomed_img

The second function, called "haar_image", is a function that is responsible to detect faces in the images and rotated images and return if it found one or not.

- **Haar image function:**
        
        def haar_image():

            detected = False

            haars_cascade = cv2.CascadeClassifier('/RoboticsAcademy/exercises/static/exercises/rescue_people_newmanager/haarcascade_frontalface_default.xml')

            img = HAL.get_ventral_image()

            zoomed_img = zoom_image(img)
        
            for angle in [0, 45, -45, 90, -90, 135, -135, 180]:  
                rotated_img = cv2.rotate(zoomed_img, angle)
                rotated_gray = cv2.cvtColor(zoomed_img, cv2.COLOR_BGR2GRAY)
            
                rotated_faces = haars_cascade.detectMultiScale(rotated_gray, scaleFactor=1.004, minNeighbors=1)
                
                for i in range(len(rotated_faces)):
                    detected = True

            return img, detected

The third function, called "repeated_persons", is a function that is responsible to detect if the person is repeated or not.

- **Repeated persons function:**

        def repeated_persons(poses, new_pose):

            repeat = False
            
            for i in range(len(poses)):
                if ((abs(poses[i][0] - new_pose[0]) <= 2) and (abs(poses[i][1] - new_pose[1]) <= 2)):
                    repeat = True
                    
            if not (repeat):
                poses.append(new_pose)
            
            return poses

Then, the spiral is done in the bucle while which is increasing the velocity in x while it call the "haar_image" to detect people and "repeated_persons".

Also when the six persons are sensed the dron will go back to the boat, landed and print/send the positions of the people.

## Critical Analysis

Finally, I have to mention that to get the best solution to this task I will have to modify the zoom factor, the scale_factor of the haar image and the velocity increase in x to get the best velocity to detect persons. All these values are importants to get the faces and don't lost it.

Also, I have established this value to compare the distance of the positions of the dron whit this values, if you change the zoom factor or the altitude of the dron you will have to change the compare value to avoid repeat faces.

Last thing to mention is that I rotated the image every 45 grades but if you want you can rotate more but it will produce a slowdown in the processing that will make you lost faces.

## Gifts and Videos

This is a gift which shows the dron doing the spiral and looking for faces.

**Gif of the obstacle avoidance:**<br>
<iframe src="https://cdn.discordapp.com/attachments/828395914145431612/1173327623766675516/gif_dron.gif?ex=65638d80&is=65511880&hm=b3f686393b8a0d3c01f27de221a3f1034ff9ac645e8f65da1cfd331e58e2a76e&" width="480" height="259" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/rs_p2.gif"></a></p>

**Simulation Video**

This video shows the simulations of the obstacle avoidance of the car while going around the circuit. 
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/V7Ljq71FJjM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  -->
 
