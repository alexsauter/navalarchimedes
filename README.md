# Naval Archimedes

## Background

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/modelboats.jpg" width="70%"></p>

This project is the basis for a programming part of the introductory subject MILM1302 at the Royal Norwegian Naval Academy. In the part called Naval Archimedes, the students have to build parts of a model boat in order to develop an understanding for the basics of naval vessels. Central for the programming part of the project is to make programming a small naval drone easy for new students. Thus, more complex parts of the programming have to be pre-prepared - which is the part that is collected in this github-project. <br><br>

The programming plattform for programming the boats by the students is Node-RED, which can be found here:<br>
https://nodered.org <br>
Installation of Node-RED, depending on the plattform, is explained here:<br>
https://nodered.org/docs/getting-started/ <br><br>

## Hardware

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/Hardware.jpg" width="100%"></p>

The control of the drones is managed by use of a Raspberry Pi (at the time <a href="https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/">Raspberry Pi 3B+</a>) as board computer. It is loaded with the standard operation system (OS). In 2024, this means Raspbian in a 32-bit version of Linux 11 (bullseye).<br><br>

The Raspberry Pi is supported by an <a href="https://store.arduino.cc/products/arduino-uno-rev3">Arduino Uno</a> in order to be able to generate better puls width modulation (PWM) signals for the motors and servos. The Arduino is pre-programmed with <a href="https://github.com/firmata/arduino">Firmata</a>, thus all programming can be done on the Raspberry Pi via Node-RED - using the palette <a href="https://flows.nodered.org/node/node-red-node-arduino">node-red-node-arduino</a> for interacting with the Arduino on a software level. The Arduino is physically connected to the Raspberry Pi via a usb-cable, using serial communication. <br><br>

As remote controll, a <a href="https://www.playstation.com/no-no/accessories/dualshock-4-wireless-controller/">Dualshock 4</a> wireless bluetooth controller is primarily used. The different buttons are recognized/are referred to as shown below.<br>
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/PS4controller.png" width="100%"></p>

The sensorsystem of the drone is basically consisting of a <a href="https://docs.px4.io/main/en/flight_controller/pixhawk4.html">Pixhawk 4</a>, which to the least is supposed to give information about the vessel's heading and GPS-positon. Furthermore, the 433 MHz digital radio transmitter, which can eigher be connected to via usb or via the Pixhawk, can be used to enable other forms of communication with the drone. Possible ways of communicating and thus controlling the drone are shown below.<br>
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/ComPorts.jpg" width="80%"></p>


## Latest version

The latest version of the project is only depending on two major external contributions, one for interaction with the Pixhawk and one for interaction with the bluetooth controller.

### Subflow for Pixhawk interaction

The subflow for interaction with the Pixhawk via serial (usb) connection is called <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/TelemetryRC.json">TelemetryRC.json</a>, including some debug and injection nodes for testing.<br>

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/TelemetryRC.png" width="100%"></p>

The Pixhawk is communicating via the mavlink protocol. In the latest version, the <a href="https://github.com/mavlink/mavlink2rest">mavlink2rest</a> program is used for communication and parcing of the mavlink messages. The location and exact name of the mavlink2rest programfile has to be adjusted within the Node-RED subflow, if not placed at the same location. When using Linux, the mavlink2rest-program get be downloaded by wget or curl.<br>
For Raspberry Pi:
  ```sh
  wget https://github.com/mavlink/mavlink2rest/releases/latest/download/mavlink2rest-arm-unknown-linux-musleabihf
  ```
For Windows:
  ```sh
  wget https://github.com/mavlink/mavlink2rest/releases/latest/download/mavlink2rest-x86_64-pc-windows-msvc.exe
  ```
Check the <a href="https://github.com/mavlink/mavlink2rest">mavlink2rest project page</a> for other or newer versions.<br><br>

Additionally, for automatic detection of the correct device, one should install globally a part of the <a href="https://serialport.io/docs/bin-list">serialport</a>-package:
  ```sh
  npm install -g @serialport/list
  ```
With this done, the node will automatically detect either the radio or the Pixhawk connected via usb. In the options of the subflow-node, one can even choose if only one of the options should be considered for auto-connect.<br>
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/TelemetryRCoptions.png" width="75%"></p>
Also the mavlink-system and mavlink-component to listen to can be selected here. However, one can still detect all other systems and components when listening at the "all"-output of the node.<br>
The datarate can be changed by setting a number of milliseconds to limit the amount of data coming in. The node can be adjusted to be working in up to 50Hz (20 ms), but the processor of the Raspberry Pi 3B+ will then use up to 25% (in addition to ~2% for the mavlink2rest-program) just for this if chosen. The default rate of 2Hz (500 ms) is gentle to the processor (~2% for the node and ~2% for the mavlink2rest-program).

### Subflow for DS4 interaction

The subflow for interaction with the Dualshock 4 controller via bluetooth connection is called <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/BluetoothDS4.json">BluetoothDS4.json</a>, including the debug and injection nodes for testing.<br>

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/BluetoothDS4.png" width="100%"></p>

For the interaction with the bluetooth controller, the <a href="https://github.com/node-hid/node-hid">node-hid</a> package is required, but in the latest version without any further dependencies.
  ```sh
  npm install node-hid
  ```
The settings.js file in the .node-red folder has to be adjusted at the functionGlobalContext position to be able to use the subflow.
  ```sh
    functionGlobalContext: {
         HID:require('node-hid')
    },
  ```
The crc-code, needed for coding the transmission of commands to the controller - but not needed for receiving buttons and such, is added as part of a function-node, taken and adapted from https://github.com/Pecacheu/dualshock/blob/master/crc.js .<br>
The original source of the parcing of the receiving buffer from the DS4 controller is based on the work of https://github.com/seidtgeist/node-ds4 .<br>
The bluetooth controller has first to be connected on an OS-level, but will then be automatically detected by the subflow-node.

## The project during the periode 2019-2023

The project before the year 2024 was parcing the mavlink protocol internally, and was thus dependent on several npm packages, preferably installed directly into the .node-red folder.
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
As the latest version is much more elegant, this first version is not recommended anymore.

## Authors
* Alexander Sauter