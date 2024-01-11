## Naval Archimedes

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/modelboats.jpg" width="70%"></p>

This project is the basis for a programming part of the subject MILM1302 at the Royal Norwegian Naval Academy. In the part called Naval Archimedes, the students have to build parts of a model boat showing their understanding of the basics of naval vessels. The central idea of the programming part of the project is to make programming a small naval drone easy for new students. <br><br>

The programming plattform for programming the boats is Node-RED, which can be found here:<br>
https://nodered.org <br>
Installation of Node-RED, depending on the plattform, is explained here:<br>
https://nodered.org/docs/getting-started/ <br><br>

## Hardware

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/Hardware.jpg" width="100%"></p>

The drones are based on a Raspberry Pi (at the time <a href="https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/">Raspberry Pi 3B+</a>), loaded with the standard operation system (OS). In 2024, this means Raspbian as a 32-bit version of Linux 11 (bullseye).<br><br>

The Raspberry Pi is supported by an <a href="https://store.arduino.cc/products/arduino-uno-rev3">Arduino Uno</a> in order to be able to generate better puls width modulation (PWM) signals for the motors and servos. The Arduino is pre-programmed with <a href="https://github.com/firmata/arduino">Firmata</a>, thus all programming can be done on the Raspberry Pi via Node-RED - using the palette <a href="https://flows.nodered.org/node/node-red-node-arduino">node-red-node-arduino</a> for interaction with the Arduino. <br><br>

As remote controll, a <a href="https://www.playstation.com/no-no/accessories/dualshock-4-wireless-controller/">Dualshock 4</a> wireless bluetooth controller is used. The different buttons are recognized as shown below.<br>
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/PS4controller.png" width="100%"></p>

The sensorsystem of the drone is basically a <a href="https://docs.px4.io/main/en/flight_controller/pixhawk4.html">Pixhawk 4</a> (can be upgraded), which to the least is supposed to give a heading and GPS-positon to the drone. Furthermore, the 433 MHz digital radio transmitter kan be used to enable other forms of communication with the drone. Possible ways of communicating and thus controlling the drone are shown below.<br>
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/ComPorts.jpg" width="100%"></p>


## After 2023

The new version of the project is based on two major contributions. For interaction with the sensorsystem, being the Pixhawk via mavlink, the mavlink2rest program is now used:<br>
https://github.com/mavlink/mavlink2rest <br>
The location and exact name of the mavlink2rest programfile has to be adjusted within the Node-RED subflow. <br><br>

Additionally, for automatic detection of the correct device, one should install globally a part of the serialport-package:
* npm
  ```sh
  npm install -g @serialport/list
  ```

For the interaction with the bluetooth controller, the node-hid package is still required, but now without any further dependencies.
* npm
  ```sh
  npm install node-hid
  ```
The crc-code, needed for coding the transmission of commands to the controller - but not needed for receiving buttons and such, as now been added as a function, taken and adapted from https://github.com/Pecacheu/dualshock/blob/master/crc.js .<br><br>

The original source of the parcing of the receiving buffer is based on the work of https://github.com/seidtgeist/node-ds4 .<br><br>

The settings.js file in the .node-red folder has to be adjusted at the functionGlobalContext position to be
  ```sh
    functionGlobalContext: {
         HID:require('node-hid')
    },
  ```

## The project during the periode 2019-2023

The project before the year 2024 was dependent on several npm packages, preferably installed directly into the .node-red folder.
* npm
  ```sh
  npm install bindings nan prebuild-install process serialport crc jspack underscore util events mavlink process
  npm install node-hid
  ```
That code depends on the inclusion of these packages into the settings.js file in the .node-red folder, depending on the location something like this:
  ```sh
    functionGlobalContext: {
         mavlink:require('/home/pi/node_modules/mavlink/src/mavlink.js'),
         HID:require('/home/pi/node_modules/node-hid/nodehid.js'),
         SerialPort:require('/home/pi/node_modules/serialport'),
         jspack:require('/home/pi/node_modules/jspack').jspack,
         underscore:require('/home/pi/node_modules/underscore'),
         events:require('/home/pi/node_modules/events'),
         util:require('/home/pi/node_modules/util'),
         crc:require('/home/pi/node_modules/crc'),
         process:require('/home/pi/node_modules/process')
    },
  ```

## Authors
* Alexander Sauter