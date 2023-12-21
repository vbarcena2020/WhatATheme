---
title: Práctica 4 - Follow Line Arduino
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/SETR_img.jpg?raw=true"
description: Programing a follow line with arduino kit by ourselves.
tags:
- Práctica 4 - Follow Line Arduino
- Post
- Sistemas Empotrados y de Tiempo Real
- SETR
- C
- Arduino
- Sensores 
---

This post will show you how my partner and I progress in our task of subject "Sistemas Empotrados y de Tiempo Real" to program and create a functional follow line with arduino:

---

# **Follow Line Arduino**
First of all, we have to assemble the arduino kit to proceed to program the line follower code. 

## Hardware
The kit we are ussing is ELEGOO Smart Robot Car V4.0 with Camera, you can buy and consult more information about all the components of the kit at this to the [official ELEGOO website](https://www.elegoo.com/en-es/products/elegoo-smart-robot-car-kit-v-4-0). To assemble the kit you can use the guide contained in the kit itself or you can consult the following [video](https://www.youtube.com/watch?v=GQi99xmohdw&ab_channel=ElegooOfficial).

The list of used hardware components and his pins are:
- Arduino UNO
- Infrared Sensors

        #define PIN_ITR20001_LEFT   A2
        #define PIN_ITR20001_MIDDLE A1
        #define PIN_ITR20001_RIGHT  A0
- Ultrasound Sensor

        #define TRIG_PIN 13  
        #define ECHO_PIN 12  
- 4 Motors

        // Enable/Disable motor control.
        //  HIGH: motor control enabled
        //  LOW: motor control disabled
        #define PIN_Motor_STBY 3

        // Group A Motors (Right Side)
        // PIN_Motor_AIN_1: Digital output. HIGH: Forward, LOW: Backward
        #define PIN_Motor_AIN_1 7
        // PIN_Motor_PWMA: Analog output [0-255]. It provides speed.
        #define PIN_Motor_PWMA 5

        // Group B Motors (Left Side)
        // PIN_Motor_BIN_1: Digital output. HIGH: Forward, LOW: Backward
        #define PIN_Motor_BIN_1 8
        // PIN_Motor_PWMB: Analog output [0-255]. It provides speed.
        #define PIN_Motor_PWMB 6
- 1 LED

        #define PIN_RBGLED 4
- ESP32

Image of our ELEGOO Smart Robot Car which we call PETER 

![](https://cdn.discordapp.com/attachments/828395914145431612/1187402201669320744/WhatsApp_Image_2023-12-21_at_11.29.07.jpeg?ex=6596c179&is=65844c79&hm=af101e346e9e243464929c09067baaafe36a2c0a5b889a92b0868d6b7b90cd5c&)

## Software 

To develop the code I have used the Arduino IDE.

### Used Libraries
- [Wifi101](https://www.arduino.cc/reference/en/libraries/wifi101/)
- [MQTT](https://github.com/adafruit/Adafruit_MQTT_Library)
- [FreeRTOS](https://github.com/feilipu/Arduino_FreeRTOS_Library)
- [FastLED](https://github.com/FastLED/FastLED)

### Planning the Implementation
We have created two different codes, one for the Arduino which is responsible for the general operation of the line follower and another for the ESP which is responsible for connecting to the internet and sending the JSON messages using MQTT.


#### States Machine
We have implemented a states machine which have a state for each infrared sensor and one if the line is lost.

- State 1: Line sense in left infrared sensor and turn left.
- State 2: Line sense in middle infrared sensor and go straight.
- State 3: Line sense in right infrared sensor and turn right.
- State 4: If all infrared sensor don't sense the line it turn the side last sensed.


#### Arduino
We use the librarie of FastLED to change the color of the Led. The color is green if the Infrared Sensors detect the line and is red if not.

In our case We have decided to use FreeRTOS for the development and execution of the program. We have implemented 4 threads with diferents priorities. The threads and their priorities created are:
- Movement of the motors - Priority 3
- Sense line with Infrared Sensors - Priority 2
- Sense distance with Ultrasound Sensor - Priority 1
- Serial Comunication with ESP32 - Priority 0

##### Threads:
Movement: in this thread we set the values of the motors depending on which infrared sensor last sensed the line.

Sense Line: in this thread we sense the line with the three Infrared Sensors.

Sense Distance: in this thread we use the ultrasound sensor to get the distance of the nearest object in front of the car and if the distance sense is between 8 and 5 cm the robot will stop moving.

Serial Cominication: in this thread we implement all or most of the communication between Arduino and the ESP.

#### ESP32
In the ESP we establish the internet connection and use the MQTT server to send to the port "/SETR/2023/4/" the messages in the JSON format indicated by the Arduino through Serial communication. 
 
    
## Videos
 

**Follow Line Arduino video**
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/tbPQeb7p6_A" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br>