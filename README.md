# A15 User Guide

### Introduction

The A15 is a flexible microcontroller, based on the Arduino Mini, and is compatible with many existing Arduino libaries and peripheral hardware.  Programming the devices is performed via the AVR In-System Programming interface.  This interface is accessible via the SPI lines and RESET line of the device.

This guide is intended to provide some initial instructions for dev environment setup and programming.

## Hardware Interface

When using the A15, there are two hardware interfaces that can access the ISP lines:
 - 6-pin pogo-pin interface.
 - ZIF connector

Once programmed, power can be applied externally via the +/- pads. (e.g. using alligator clips to power a demo)

See the following image:

![power connections](./img/power_connections.png)

Left: ZIF connection. Center: POGO connection. Right: Power pads.

__NOTE:__ To learn more about both of these connections, see the A15 Technical Specification Guide.

# Programming A15 with Arduino Uno Guide

Although there are dedicated ISP Programmers, a common and inexpensive method to use the interface is by using another Arduino.  The Arduino IDE provides integrated support for this method of device programming.

### Preperation - Configuring the Arduino UNO
Before an Arduino can be used as an ISP programmer, it must first be programmed with a translation program that communicates via serial on to the host interface and SPI to the target device.  Within the provided examples, Arduino has a sketch called *ArduinoISP*.
![arduino isp sketch](./img/arduino_isp_setup1.png)

Insert the Arduino (for this example the UNO), configure the proper port/board setting, and upload the sketch.
![arduino uno settings](./img/arduino_isp_setup2.png) ![arduino upload](./img/arduino_isp_setup3.png)

### Step One - Connect The A15 Via A Programmer

Once setup, the Zero Insertion Force (ZIF) interface provides a convenient development environment that enables programming and device testing.

The A15 uses a 30-pin 0.5mm pitch ZIF interface.  An easy to use break-out board for development can be found, [here](https://www.amazon.com/gp/product/B07RWNFKCR/).  Other ZIF connectors should work well, though we have found a need to insert a spacer to make good contact with other connectors.

![fpc_30p](./img/fpc_30p.jpg)

__NOTE:__ Most ZIF connectors have contacts only on the bottom requiring the user to place the A15 device face down in the connector. If a ZIF connector with contacts on both sides or the top of the connector is used, and the device is being placed face up in the connector the pin mapping on the ZIF connector should be reversed.

Once you have your A15 hooked up to a ZIF connector, you are ready to start connecting pins.

Connections:

* Arduino GND -> ZIF 1,2,3
* Arduino Vin -> ZIF 4,5,6
* Arduino 10 -> ZIF 13 (Reset)
* Arduino 11 -> ZIF 24 (MOSI)
* Arduino 12 -> ZIF 23 (MISO)
* Arduino 13 -> ZIF 22 (SCK)

![A15 connections](./img/a15_connections.PNG)

The secondary 6-pin POGO interface is useful for embedded devices where connection to the ZIF connector is not practical.  Source parts can be purchased from a variety of vendors, a fully assembled 6-pin connector can be found, [here](https://www.segger.com/products/debug-probes/j-link/accessories/adapters/6-pin-needle-adapter/).  An example of this interface can be seen in the following images:

![pogo connections side](./img/pogoconnection_side.jpg)

![pogo connections top](./img/pogoconnection_top.jpg)

The POGO connection, ZIF connection, and front facing pads connections are all SPI - thus they follow the same general guidelines.

![pogo connections diagram](./img/pogoconnection_diagram.PNG)

Once you have successfully connected your programming interface, you are ready for the next step.

### Step Two - Load the NextFlex A15 Board into the Arduino IDE

Once you have connected your Arduino to the A15, then you are ready to install a bootloader.

The first step of this is to download the NextFlex A15 board package. 
In Arduino, navigate to Files > Preferences > Additional Boards Manager URLSs.
Paste the following URL to the open space: ```https://raw.githubusercontent.com/rmcmanus-nf/a15/master/package_A15_index.json```

![preferences json](./img/preferences_json.PNG)

This is the link to a JSON that will allow Arduino to install the necessary files for the A15 bootloader.
Now, navigate to Tools > Board: > Boards Manager... In the top search bar, search for "A15". You should see the following option:

![boards manager](./img/boards_manager.PNG)

Your dropdown window should now show the NextFlex A15 under the list of available boards.

![boards nextflex](./img/boards_nextflex.PNG)

Congratulations! You have added the NextFlex A15 Board into the Arduino IDE.

### Step Three - Upload A Program

Once you have selected the NextFlex A15 Board, you are ready to upload a program.  If you are using an Arduino to program via the ISP, you will need to specify the port of the connected Arduino, and the programmer as *Arduino as ISP*, as shown here:
![arduino upload](./img/arduino_isp_setup4.png)

Create a new Sketch, paste the following code in Arduino:

```cpp
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  
  digitalWrite(9, HIGH);
  digitalWrite(10, LOW);
  delay(1000);                    
  digitalWrite(LED_BUILTIN, LOW);  
  digitalWrite(9, LOW);
  digitalWrite(10, HIGH);  
  delay(1000);                
}
```

To upload, navigate to *Sketch > Upload Using Programmer*.  Do NOT use the *Upload* button or icon.
Your Arduino should blink its LEDs. 

![arduino upload using programmer](./img/upload_using_programmer.png)

Congratulations, you have uploaded a program to the A15!
