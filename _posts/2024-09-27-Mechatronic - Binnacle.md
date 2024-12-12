---
title: Mechatronic - Binnacle
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/Mecatronica_img2.jpg?raw=true"
description: Binnacle of the Mechatronic subject.
tags:
- Mechatronic - Binnacle
- post
- Mecatronica
- Binnacle
- Python
- C++
- Arduino
- Inkscape
- Githuh
- MD / Markdown 
- VSCode
- Croquis
- FreeCAD
---

This post will show you how I progress in the subject "Mecatronica" learning new habilities and programs to do different things as 2D design:

---
 
# **L1 and L2 Learning VSCode and Github**
In this labs we learn how to used the basics comands of github to update all our content to the github repositories. Also we learn how to do that in VSCode in a graphic form. I will use the terminal to update the code because I found it more simple due to I get used to it all these years. The mor important comands are:

To clone in local the repository:

        $ git clone [url of the repository]

To know the local status of the git repository:

        $ git status

To add the new files, or the changes done in the other files:

        $ git add [files names]

To add a comment and commit the changes done:

        $ git commit -m 'comentario'

To push the changes to the online repository:

        $ git push

To pull the changes form the online repository:

        $ git pull

To merge the changes or two branches use:

        $ git merge [branches]

To create a new branch:

        $ git branch [name of the new branch]

To change between branches:

        $ git checkout [name of the branch]

To create and change to the new branch:

        $ git checkout -b [name of the new branch]

These are the basics commands I used.

# **Learning Inksacape 2D Design**
Fristly, I download the Inkscape program. 
Then after learning how to use the program and design some things in 2D I try to create a simple design of the Proyect we are going to create "Proyecto Mano de Zeus". 

The design is the following:

<br>
<image src="https://github.com/vbarcena2020/Mecatronica-2024-2025/blob/main/L5/Mano.svg?raw=true"></image> 
<br>


# **Learning FreeCAD 3D Design**
After learning how to create a simple model of the hand in Inksacape in 2D I try to create a simple structure of all the pieces of which the hand that we want to design and create is composed of. The 

La estructura se encuentra en el siguiente link: ![Mano.FCStd](https://github.com/mpancracio2020/Mecatronica-Proyecto/tree/main/images/Mano.FCStd)

<br>
<image src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/simple_mano_3d.png?raw=true"></image> 
<br>

En cuanto al resto de diseño 3D hemos utilizado Ultimaker Cura para diseñar las piezas basandonos en el proyecto original del cual hemos sacado la idea. Hemos modificado los archivos (.lts) de los componentes aportando diferentes valores para obtener la mejor impresión de las piezas en nuestras impresoras 3D.

# **Making Electronic Design**
To create and design the electronic circuit, fristly, we have to decide which components we are going to need and use.

For the control of the hardware we are going to use an Arduino UNO which can be replace by an Arduino Mega if we want to add more components. 

<br>
<image src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/arduinos.jpg?raw=true"></image> 
<br>

The rest of components are six servos per hand, one for each finger and other for the wrist. This 360 servos model we are going to use are the tower pro mg996r.

<br>
<image src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/servo.jpg?raw=true"></image> 
<br>

And to connect all the servos and the arduino we use cables and a protoboard.

To visualize the design I use Fritzing and app which allow us create electronic designs, and this is our:

<br>
<image src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/fritzing_design.png?raw=true"></image> 
<br>

After trying the code with this connection the servos move to slow or doesn't have the strong to move the fingers. This happend because the 5V that gives the arduino to the servos is not enough to move the fingers so to fix it I am gonna add a battery to provaid Voltage to the servos.

So the new design is the following:

<br>
<image src="https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/fritzing_design_2.png?raw=true"></image> 
<br>

