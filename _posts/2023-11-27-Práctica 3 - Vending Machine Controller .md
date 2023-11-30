---
title: Práctica 3 - Vending Machine Controller
layout: post
post-image: "https://github.com/vbarcena2020/My_personal_page/blob/master/assets/images/SETR_img.jpg?raw=true"
description: Programing a vending machine controller by myself.
tags:
- Práctica 3 - Vending Machine Controller
- Post
- Sistemas Empotrados y de Tiempo Real
- SETR
- C
- Arduino
- Sensores 
---

This post will show you how I progress in my task of subject "Sistemas Empotrados y de Tiempo Real" to program and create a simplified vending machine controller:

---

# **Vending Machine Controller**
First of all, I decided to separate this task in two, the hardware and software. 

## Hardware
The list of implemented hardware components are:
- Arduino UNO
- 2 Normal LEDS (LED1, LED2)
- Button
- Ultrasound Sensor
- DHT11 temperature/humidity sensor
- Joystick
- LCD
- Potentiometer

Before starting to assemble the entire circuit and all the components at once, I am going to check the status of all the sensors and actuators while I take the opportunity to understand how they work so that I can then use them more easily and in a better way.

To assemble the hardware I have used fritzing, downloading the necessary libraries to have the specific components.

### Component Status Check

These are the assemblies and codes that I have used for each hardware component of those previously mentioned.

#### 2 Normal LEDS
The implementation of two normal leds is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1178655697546788916/leds.png?ex=6576efa8&is=65647aa8&hm=b7002a9070b2b988df4a4fa8503cda3b7b8dfd461da980dd4d03ecd03b65340f&)

The tried code for the two normal leds is:

        const int ledPin1 = A2; 
        const int ledPin2 = A3; 

        void setup() {
            pinMode(ledPin1, OUTPUT);
            pinMode(ledPin2, OUTPUT);
        }

        void loop() {
            analogWrite(ledPin1, 255);  
            analogWrite(ledPin2, 255);   
            delay(1000);                  
            
            analogWrite(ledPin1, 0);
            analogWrite(ledPin2, 0);
            delay(1000); 
        }

<br>

#### Button
The implementation of the button is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1178655696452059146/boton.png?ex=6576efa8&is=65647aa8&hm=c4177fa40ae5c66ffb9746bd428f27c38d6f51017bbda04848a5472e03f582ed&)

The tried code of the button is:

        const int buttonPin = 7;
        int button_state = 0;

        void setup() {
            pinMode(buttonPin, INPUT); 
            Serial.begin(9600);
        }

        void loop() {
            int current_button_state = digitalRead(buttonPin);

            if (current_button_state != button_state) {
                button_state = current_button_state;

                if (button_state == LOW) {
                    Serial.println("Button pressed");
                }
            }
        }

<br>

#### Ultrasound Sensor
The implementation of a ultrasound sensor is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1178655697815228476/ultrasonidos.png?ex=6576efa8&is=65647aa8&hm=34701866c8612f22ae30005ab69c5360de83991a341f3cba4df021eb5f46c971&)

The tried code a ultrasound sensor is:

        #define trigPin 9
        #define echoPin 8

        void setup() {
            Serial.begin(9600);
            pinMode(trigPin, OUTPUT);
            pinMode(echoPin, INPUT);
        }

        void loop() {
            long duration, distance;
            
            digitalWrite(trigPin, LOW);
            delayMicroseconds(4);
            digitalWrite(trigPin, HIGH);
            delayMicroseconds(10);
            digitalWrite(trigPin, LOW);
            
            duration = pulseIn(echoPin, HIGH);
            
            distance = duration * 10 / 292 / 2;   

            Serial.print("Distance: ");
            Serial.print(distance);
            Serial.println(" cm");
            
            delay(100);
        }

<br>

#### DHT11 temperature/humidity sensor
The implementation of a dht11 is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1179015664187478086/dht11.png?ex=65783ee7&is=6565c9e7&hm=952ffda34f16e8a4b40715aabce39c9bfc01d00490d082bc84470f275b7dea46&)

The tried code of a dht11 is:

        #include <DHT.h>

        #define DHTPIN 10
        #define DHTTYPE DHT11
        
        DHT dht(DHTPIN, DHTTYPE);
        
        void setup() {
            Serial.begin(9600);
            dht.begin();
        }
        
        void loop() {
            delay(5000);
            
            float h = dht.readHumidity();
            float t = dht.readTemperature();
            
            if (isnan(h) || isnan(t)) {
                Serial.println("Error obteniendo los datos del sensor DHT11");
                return;
            }
            
            Serial.print("Humedad: ");
            Serial.print(h);
            Serial.print(" %\t");
            Serial.print("Temperatura: ");
            Serial.print(t);
            Serial.print(" *C ");
        }

<br>

#### Joystick
The implementation of a joystick is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1179015664502046740/joystick.png?ex=65783ee7&is=6565c9e7&hm=1d79cbd5ce2a1d27b9d4d5a0b56baecb4166b9e47852ceafe49729be87fef8d5&)

The tried code of a joystick is:

        const int pinJoyX = A0;
        const int pinJoyY = A1;
        const int pinJoyButton = 13;

        void setup() {
            pinMode(pinJoyButton , INPUT_PULLUP);  //activar resistencia pull up 
            Serial.begin(9600);
        }

        void loop() {
            int Xvalue = 0;
            int Yvalue = 0;
            bool buttonValue = false;
            int move = NOT_UP_DOWN;

            Xvalue = analogRead(pinJoyX);
            delay(100);
            Yvalue = analogRead(pinJoyY);
            buttonValue = digitalRead(pinJoyButton);
            Serial.print("Rx: ")
            Serial.println(Xvalue);
            Serial.print("Ry: ")
            Serial.println(Yvalue);
            Serial.print("State: ")
            Serial.println(buttonValue);
            
            delay(100);
        }

<br>

#### LCD
The implementation of a lcd is as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1178655697274142740/lcd.png?ex=6576efa8&is=65647aa8&hm=3e1152c6236bf8efd10ad6e83f8ba5476cfeb97ca16660558af49c0ce2380553&)

The tried code of a lcd is:

        #include <LiquidCrystal.h> 
        int Contrast=75;
        LiquidCrystal lcd(12, 11, 5, 4, 3, 2);  

        void setup() {
            analogWrite(7,Contrast);
            lcd.begin(16, 2);
        } 

        void loop() { 
            lcd.setCursor(0, 0);
            lcd.print("Hola");
            
            lcd.setCursor(0, 1);
            lcd.print("Mundo");
        }

<br>

### Planning the Implementation

To start assembling I have to keep in mind that I need to connect fifteen pins in total to the Arduino UNO without counting the Vcc (5V) pin and Gnd pin. Of the fifteen pins, two are analog for the joystick while the others are digital, but because there are only twelve digital pins that are fully operational and do not affect operation, I have decided to put both LED pins in analog.

I have also taken into account that the pin of the button that is going to be treated as an interruption has been set to digital pin 2.

Finally, the pins are as follows:

![](https://cdn.discordapp.com/attachments/828395914145431612/1179463674574946405/tabla.png?ex=6579e025&is=65676b25&hm=670ba995ca74d67782b84c987c386d58dde12eab0120a74a84fcd7fb27300349&)

The finally assembled circuit has the following form:

![](https://cdn.discordapp.com/attachments/828395914145431612/1179015664816635965/p3.png?ex=65783ee7&is=6565c9e7&hm=9b94cbb23beace79b824fa78dbffa229e47ae33c966b31106d427faa9a7eb364&)

![](https://cdn.discordapp.com/attachments/828395914145431612/1179466248883544205/1701277041543.jpg?ex=6579e28b&is=65676d8b&hm=0b48e2590716eac2559656fae63958ed7056187db372774c6b29a53b0b220f2b&)
![](https://cdn.discordapp.com/attachments/828395914145431612/1179466248174710825/1701277041536.jpg?ex=6579e28a&is=65676d8a&hm=3fcace4ac6372835e5e8c168ef108b2cc23d56dc124448f391def5b39565462f&)

## Software 

To develop the code I have used the Arduino IDE.

### Used Libraries
- LiquidCrystal: <https://www.arduino.cc/en/Reference/LiquidCrystal>
- ArduinoThread: <https://www.arduino.cc/reference/en/libraries/arduinothread/>
- Watch Dog: <https://create.arduino.cc/projecthub/rafitc/what-is-watchdog-timer-fffe20>
- DHT-sensor-library: <https://github.com/adafruit/DHT-sensor-library>

### Planning the Implementation

In my case I have decided to propose this practice as a state machine with a total of 3 states, "arranque", "servicio" and "admin", each with its necessary substates.

In addition I have implemented two threads and an interrupt. Also I use watchdog to avoid locks.

#### State 1 "arranque"

State one or "arranque" is responsible for turning blue LED 1 on and off three times at intervals of one second, while displaying "Cargando..." on the LCD.

When it goes off for the third time the LED goes to state 2 or "servicio".

#### State 2 "servicio"

This state has four substates.

Substate 1: is responsible for displaying the temperature and humidity on the LCD for 5 seconds if it detects a person less than one meter away, otherwise it displays "ESPERANDO CLIENTES" on the LCD.

Substate 2: is responsible for displaying the products on the LCD until the joystick button is detected.

Substate 3: is responsible for displaying "Preparando Cafe …" on the LCD for a random time between 4 and 8 seconds.

Substate 4: is responsible for displaying "RETIRE BEBIDA" on the LCD for 3 seconds and returning to the substate 1

#### State 3 "admin"

This state has six substates.

Substate 1: is responsible for displaying the admin menu on the LCD and turning on both LEDs.

Substate 2: is responsible for call the menu function (show temperature, show distance, show counter or the next states) or state when the the joystick button is detected.

Substate 3: is responsible for displaying the products on the LCD until the joystick button is detected.

Substate 4: is responsible for displaying the product selected and his price and you can modify it with the joystick.

Substate 5: is responsible for displaying the other products list on the LCD until the joystick button is detected and add this new product to the products and remove it to the other products list.

Substate 6: is responsible for displaying the products list on the LCD until the joystick button is detected and remove this product to the products list and add it to the other products list.

In all substates except the substate 1 if the joystick detects that it has been moved to the right, it will return to the previous state.

#### Threads

I have implemented two threads and a controler. 

The first one is responsable to sense the distace of the ultrasounds.

The second one is responsable to get the moves of the joystick and the signals of the joystick button.

#### Interrupt

I have create a interrupt to detect the CHANGES in the button signal. 

It is responsable to calculate the time the button is pressed. 

During the entire execution of the program, if you press it for more than 5 seconds, it will change to the "admin" state or it will return to the "servicio" state if it was already in the "admin" state.

At the same time, if it is in the "servicio" state and a person has already been detected, the "servicio" state can be restarted if it is maintained for 2 to 3 seconds.


## Videos

**Coffee Vending Machine Controller video**

<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/dnSEKLYZkVA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br>

**Coffee Vending Machine Controller Reduced video**

<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/BR8mtPs9ICw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 