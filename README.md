About?
HuskyLens is an AI-powered vision sensor designed for projects involving robotics, artificial intelligence, and automation. It is developed by DFRobot and offers several built-in AI functions, including face recognition, object tracking, object recognition, line following, color recognition, and tag (QR code) recognition. 

HuskyLens is user-friendly, featuring a touchscreen interface that allows for easy interaction and configuration without needing a computer. It supports various communication protocols such as UART, I2C, and SPI, making it compatible with different microcontrollers like Arduino, Raspberry Pi, and ESP32.

This sensor is particularly useful for hobbyists, educators, and developers who want to add AI-based vision capabilities to their projects without delving into complex programming.


# Starting
- Please download and install the [HUSKYLENS Library](https://github.com/user-attachments/files/16574016/HUSKYLENSArduino-master.zip) first.

- Unzip the file, then copy the folder to the "libraries" folder of the Arduino IDE. Then check whether the folder name is "HUSKYLENS". If not, please change it as "HUSKYLENS". Please note that the library file name must be HUSKYLENS.

<img src= "https://github.com/user-attachments/assets/0bf10b87-2f47-48c2-ab1a-68f5a396198b" alt= "img" width= 400>

- All .h files and .cpp files must in the root directory of the "HUSKYELSN" folder. <br>
<img src= "https://github.com/user-attachments/assets/984500e0-0f28-46d3-865a-2931faeaee80" alt= "img" width= 400>

# Requirements
## Hardware
- DFRduino UNO R3 (or similar) x 1
- HUSKYLENS x 1
- M-M/F-M/F-F Jumper wires
## Software
- Arduino IDE(version 1.8.x is recommended)
- Download and install the HUSKYLENS Library (About how to install the library?)

# UART Mode(SoftwareSerial)
<img src= "https://github.com/user-attachments/assets/c8bcbd34-3a86-4201-9b2c-9252c751a71c" alt= "img" width= 400>

> [!NOTE]
> For "HuskyLens Protocol Setting" You need to set the protocol type of HuskyLens. The protocol should be 'Serial 9600'. Of course, your can adopt the Auto Detect protocol, which is easy-to-use and convenient.

# Simple Code 

``` CPP
#include "HUSKYLENS.h"
#include "SoftwareSerial.h"

HUSKYLENS huskylens;
SoftwareSerial mySerial(10, 11); // RX, TX
//HUSKYLENS green line >> Pin 10; blue line >> Pin 11
void printResult(HUSKYLENSResult result);

void setup() {
    Serial.begin(115200);
    mySerial.begin(9600);
    while (!huskylens.begin(mySerial))
    {
        Serial.println(F("Begin failed!"));
        Serial.println(F("1.Please recheck the \"Protocol Type\" in HUSKYLENS (General Settings>>Protocol Type>>Serial 9600)"));
        Serial.println(F("2.Please recheck the connection."));
        delay(100);
    }
}

void loop() {
    if (!huskylens.request()) Serial.println(F("Fail to request data from HUSKYLENS, recheck the connection!"));
    else if(!huskylens.isLearned()) Serial.println(F("Nothing learned, press learn button on HUSKYLENS to learn one!"));
    else if(!huskylens.available()) Serial.println(F("No block or arrow appears on the screen!"));
    else
    {
        Serial.println(F("###########"));
        while (huskylens.available())
        {
            HUSKYLENSResult result = huskylens.read();
            printResult(result);
        }    
    }
}

void printResult(HUSKYLENSResult result){
    if (result.command == COMMAND_RETURN_BLOCK){
        Serial.println(String()+F("Block:xCenter=")+result.xCenter+F(",yCenter=")+result.yCenter+F(",width=")+result.width+F(",height=")+result.height+F(",ID=")+result.ID);
    }
    else if (result.command == COMMAND_RETURN_ARROW){
        Serial.println(String()+F("Arrow:xOrigin=")+result.xOrigin+F(",yOrigin=")+result.yOrigin+F(",xTarget=")+result.xTarget+F(",yTarget=")+result.yTarget+F(",ID=")+result.ID);
    }
    else{
        Serial.println("Object unknown!");
    }
}
```

# Demo

