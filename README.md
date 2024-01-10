[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

## Naval Archimedes

This idea of the project is to make programming a small naval drone easy for new students. The programming plattform for programming the boats is Node-RED:
https://nodered.org

Install Node-RED depending on your plattform as shown:
https://nodered.org/docs/getting-started/

The drones are based on a Raspberry Pi (at the time 3B+):
https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/
Supported by Arduino Uno for PWM, pre-programmed with Firmata:
https://store.arduino.cc/products/arduino-uno-rev3
https://github.com/firmata/arduino

The remote controll used is a Dualshock 4 wireless bluetooth controller:
https://www.playstation.com/no-no/accessories/dualshock-4-wireless-controller/

The sensorsystem of the drone is basically a Pixhawk 4 (can be upgraded):
https://docs.px4.io/main/en/flight_controller/pixhawk4.html

## 2019-2023

The project before the year 2024 was dependent on several npm packages, preferably installed directly into the .node-red folder.
* npm
  ```sh
  npm install bindings nan prebuild-install process serialport crc jspack underscore util events mavlink process
  npm install node-hid
  ```

## After 2023

The new version of the project is based on two major contributions. For interaction with the sensorsystem, being the Pixhawk via mavlink, the mavlink2rest program is now used:
https://github.com/mavlink/mavlink2rest
The location and exact name of the mavlink2rest programfile has to be adjusted within the Node-RED subflow.
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
The crc-code, needed for coding the transmission of commands to the controller - but not needed for receiving buttons and such, as now been added as a function, taken and adapted from 
https://github.com/Pecacheu/dualshock/blob/master/crc.js
The original source of the parcing of the receiving buffer is based on the work of 
https://github.com/seidtgeist/node-ds4
