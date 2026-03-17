# Naval Archimedes as of 2025 and beyond

## Background
This new version is made for and tested on a Raspberry Pi 5 setup, as an upgrade from the former RPi3B+ version.

The code and setup may often be similar or the same as for the previous version, but is often optimized a bit - and adjusted where dependencies have changed.

In this intermediate development-step, the original links are kept the same - so the new versions are collected in the folder. In a future release, this will be the main version.

## Hardware

In addition to the original design (as of now shown at the <a href="https://github.com/alexsauter/navalarchimedes/tree/main">main</a> page), some parts have been upgraded and added.

### LiPo battery
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/blob/main/RPi5dev/figs/LiPoBattery.jpg" width="80%"></p>
As the Raspberry Pi 5 requires more power than the previous version, the new version is powered by a <a href="https://gensace.de/products/b-60c-5000-2s1p-bashing-hc112-deans-gt">Lipo Battery</a> instead of the NiMH battery that was used before. The new battery delivers more power, but a higher degree of caution has to be used charging and using the battery. The power output of the electric motors also changed, but this is (so far) not shown here.

Power is now distributed via all three ESCs, instead of only one of them.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/blob/main/RPi5dev/figs/PowerDistribution.png" width="100%"></p>



## Authors
* Alexander Sauter