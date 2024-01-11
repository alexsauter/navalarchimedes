## Naval Archimedes

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/modelboats.jpg" width="70%"></p>

This project is the basis for a programming part of the subject MILM1302 at the Royal Norwegian Naval Academy. In the part called Naval Archimedes, the students have to build parts of a model boat showing their understanding of the basics of naval vessels. The central idea of the programming part of the project is to make programming a small naval drone easy for new students. <br><br>

The programming plattform for programming the boats is Node-RED, which can be found here:<br>
https://nodered.org <br>
Installation of Node-RED depending on the plattform is explained here:<br>
https://nodered.org/docs/getting-started/ <br><br>

## Hardware

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/Hardware.jpg" width="100%"></p>

The drones are based on a Raspberry Pi (at the time 3B+; https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/), loaded with the standard operation system. In 2024, this means Raspbian as a 32-bit version of Linux 11 (bullseye).<br><br>

The Raspberry Pi is supported by an Arduino Uno (https://store.arduino.cc/products/arduino-uno-rev3) for better puls width modulation (PWM) signals for the motors and servos. The Arduino is pre-programmed with Firmata (https://github.com/firmata/arduino). <br><br>

The remote controll used is a Dualshock 4 wireless bluetooth controller:<br>
https://www.playstation.com/no-no/accessories/dualshock-4-wireless-controller/ <br><br>

The sensorsystem of the drone is basically a Pixhawk 4 (can be upgraded):<br>
https://docs.px4.io/main/en/flight_controller/pixhawk4.html <br><br>

## 2019-2023

The project before the year 2024 was dependent on several npm packages, preferably installed directly into the .node-red folder.
* npm
  ```sh
  npm install bindings nan prebuild-install process serialport crc jspack underscore util events mavlink process
  npm install node-hid
  ```

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
The crc-code, needed for coding the transmission of commands to the controller - but not needed for receiving buttons and such, as now been added as a function, taken and adapted from <br>
https://github.com/Pecacheu/dualshock/blob/master/crc.js <br><br>

The original source of the parcing of the receiving buffer is based on the work of<br>
https://github.com/seidtgeist/node-ds4 <br><br>


## Authors
* Alexander Sauter