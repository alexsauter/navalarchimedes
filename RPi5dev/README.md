# Naval Archimedes as of 2025 and beyond

## Background
This new version is made for and tested on a Raspberry Pi 5 setup, as an upgrade from the former RPi3B+ version.

The code and setup may often be similar or the same as for the previous version, but is often optimized a bit - and adjusted where dependencies have changed.

In this intermediate development-step, the original links are kept the same - so the new versions are collected in the folder. In a future release, this will be the main version.

## Hardware

In addition to the original design (as of now shown at the <a href="https://github.com/alexsauter/navalarchimedes/tree/main">main</a> page), some parts have been upgraded and added.

### LiPo battery
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/LiPoBattery.png" width="50%"></p>
As the Raspberry Pi 5 requires more power than the previous version, the new version is powered by a <a href="https://gensace.de/products/b-60c-5000-2s1p-bashing-hc112-deans-gt">Lipo Battery</a> instead of the NiMH battery that was used before. The new battery delivers more power, but a higher degree of caution has to be used charging and using the battery. The power output of the electric motors also changed, but this is (so far) not shown here.<br><br>

Power is now distributed via all three ESCs, instead of only one of them.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/PowerDistribution.png" width="80%"></p>

### Laser turret with camera
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/Turret.jpg" width="35%"></p>
There has been a camera-option before, but this has now been improved significantly (see programming in later section). The new camera is a Raspberry Pi camera module version 3.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RPiCamereModuleV3.png" width="15%"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/LasterPointer.png" width="15%"></p>
Also, there was earlier no common option of directing the camera. To small servos where added to add that feature. As a small "proof-of-principle", there has also been added a small laser - a show of principle how an effector can be added to a drone like this. The laser is low power an can directly be used via a 5V digital output pin of the Arduino.


### RC solution
Finally, a proper RC controller and receiver was added to the options to control the drone.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RCradio.jpg" width="15%"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RCreceiver.jpg" width="15%"></p>
The receiver is read in via the Pixhawk, using the MAVlink protocol as before.

## Software solutions for RPi 5

### Subflow for DS4 interaction


## Authors
* Alexander Sauter