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
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RPiCamereModuleV3.png" width="15%">&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/LasterPointer.png" width="15%"></p>
Also, there was earlier no common option of directing the camera. To small servos where added to add that feature. As a small "proof-of-principle", there has also been added a small laser - a show of principle how an effector can be added to a drone like this. The laser is low power an can directly be used via a 5V digital output pin of the Arduino.

### RC solution
Finally, a proper RC controller and receiver was added to the options to control the drone.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RCradio.jpg" width="20%">&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RCreceiver.jpg" width="35%"></p>
The receiver is read in via the Pixhawk (SBUS), using the MAVlink protocol as before.

### Raspberry Pi Connect
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/RPiConnect.png" width="30%"></p>
The Raspberry Pi 5 comes additionally with the option to remote control the unit via Raspberry Pi Connect. This is a easy access way getting to work with programming of the drone without much prior knowledge of data systems.

## Software solutions for RPi 5

### Subflow for DS4 interaction
The bluetooth controller is still working. As it is dependent on the node-hid package (see <a href="https://github.com/alexsauter/navalarchimedes/tree/main">main</a> for integration of that to Node-RED), this should continue working as long as node-hid is not changing significantly or being deprecated.

<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/figs/BluetoothDS4.png" width="100%"></p>

With only minor changes, the subflow for interaction with the Dualshock 4 controller via bluetooth connection is called <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5BluetoothDS4.json">RPi5BluetoothDS4.json</a>, including the debug and injection nodes for testing.

### Integration of camera
There are two easy to use github-projects for integrating the camera:
* <a href="https://github.com/monkeymademe/CamUI">CamUI</a>
* <a href="https://github.com/bluenviron/mediamtx">MediaMTX</a> (see also <a href="https://mediamtx.org/">mediamtx.org</a>)
Both solutions have been tested and can work. CamUI is a nice solution where one can change the camera-settings live. MediaMTX is better after configuration for streaming in different ways, with low latency. Both ways are shortly presented, but the MediaMTX solution is recommended for such a drone-solution - as it is flexible but still high performance in terms of resolution and latency.</br></br>
#### CamUI
CamUI is based on flask, picamera2 and python. All of those should be available on a standard Raspberry Pi 5. Possibly update the OS before downloading from github:
```sh
sudo apt update && sudo apt upgrade -y
```
Clone the repository to the Raspberry Pi:
```sh
git clone https://github.com/monkeymademe/picamera2-WebUI.git
```
Enter the directory:
```sh
cd picamera2-WebUI
```
Run the application through
```sh
python app.py
```
and access the web interface through your browser: http://Your IP:8080/.</br></br>
If the mDNS is working, you should be able to find the page via <a href="http://boatpi01.local:8080/">http://boatpi01.local:8080/</a>, just change the number for the specific boat.</br></br>
The program kan also be started as a service, e.g. "picamera2-webui.service", in a form something like this: 
```sh
[Unit]
Description=CamUI Server
After=network.target
[Service]
Type=simple
ExecStart=/usr/bin/python /home/pi/CamUI/app.py
Restart=always
[Install]
WantedBy=multi-user.target
```
However, if this will use the camera continuously - using a significantly higher amount of power continuously. A better solution is then the <a href="https://github.com/alexsauter/navalarchimedes/tree/main/RPi5dev#mediamtx">MediaMTX</a> solution, which only will use the camera if watched (if setup properly).</br></br>
CamUI can also be integrated into Node-RED, both starting the camera-feed and integration into the dashboard.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/SubflowCamera.png" width="100%"></p>
This example subflow can be downloaded here: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5CamUI.json">RPi5CamUI.json</a>.

#### MediaMTX
MediaMTX is also mentioned among the solutions recommended on <a href="https://www.raspberrypi.com/documentation/computers/camera_software.html">raspberrypi.com</a>. The process is similar to CamUI, but without python.</br></br>
Make first a folder for mediamtx. Its content can be deleted in the end, this will only be a temporary folder during setup.
```sh
mkdir mediamtx
cd mediamtx
```
Then download the newest version compatible with Raspberry Pi 5. At the time of writing this, it would be v1.16.3 for linux (arm). Download and extract the files:
```sh
wget https://github.com/bluenviron/mediamtx/releases/download/v1.16.3/mediamtx_v1.16.3_linux_arm64.tar.gz
tar -xvzf mediamtx_v1.16.3_linux_arm64.tar.gz
```
You can make a backup of the settings-file, just in case:
```sh
cp mediamtx.yml mediamtx.yml_orig
```
Change the settings in the file mediamtx.yml to:
```sh
paths:
  cam:
    source: rpiCamera
    (...)
    sourceOnDemand: yes
```
or make the RPi Camera the default path:
```sh
pathDefaults:
    source: rpiCamera
    (...)
    sourceOnDemand: true
(...)
paths:
    cam:
```

To test, start mediamtx in that folder (it will use the settings-file if found):
```sh
./mediamtx
```
To view the camera-stream in a web-browser, use the address http://ip-addr-of-MediaMTX-server:8889/cam. Making use of mDNS here, you can fine you drone's camera-feed using <a href="http://boatpi01.local:8889/cam">http://boatpi01.local:8889/cam</a>, just change the number of the boat according to your drone.</br></br>
If you are satisfied with your settings, you can add a mediamtx-service that starts with the OS. See <a href="https://mediamtx.org/docs/other/start-on-boot">https://mediamtx.org/docs/other/start-on-boot
</a> for possible updates. But in short, copy the server executable and configuration into global folders:
```sh
sudo cp mediamtx /usr/local/bin/mediamtx
sudo cp mediamtx.yml /usr/local/etc/mediamtx.yml
```
Create a systemd service:
```sh
sudo tee /etc/systemd/system/mediamtx.service >/dev/null << EOF
[Unit]
Description=MediaMTX streaming server
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/mediamtx /usr/local/etc/mediamtx.yml
User=pi
Group=pi
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```
Enable a wait-online service:
```sh
sudo systemctl enable systemd-networkd-wait-online.service
```
Enable and start the service:
```sh
sudo systemctl daemon-reload
sudo systemctl enable mediamtx
sudo systemctl start mediamtx
```
You should then again be able to see the camera-feed when using the same address as before: <a href="http://boatpi01.local:8889/cam">http://boatpi01.local:8889/cam</a>, just change the number of the boat according to your drone.</br></br>
If necessary, check the status of the service:
```sh
systemctl status mediamtx
journalctl -u mediamtx -f
```
Or restart the service:
```sh
sudo systemctl daemon-reload
sudo systemctl restart mediamtx
```
This MediaMTX solution is then not necessarily integrated into Node-RED, as it runs independently from the control-level. However, a similar setup as for the <a href="https://github.com/alexsauter/navalarchimedes/tree/main/RPi5dev#camui">CamUI</a> solution can be used if needed. Use the subflow at the end of that section, just change the address to the one you want to use.</br></br>
Interestingly, the MediaMTX solution is not only supporting the WebRTC access mentioned so far, but allows also many other ways of accessing the video-feed. One other that is worth menioning, maybe for use in VLC-player, is RTSP.To view the stream using a media player application with the RTSP protocol, use the address rtsp://ip-address-of-MediaMTX-server:8554/cam, in this case <a href="rtsp://boatpi01.local:8554/cam">rtsp://boatpi01.local:8554/cam</a> - just with the right number for your drone. Further options are available through these other access ways, like recording of the video-feed.</br></br>
Finally, make notice of that this solution doesn't require resources as long as not video-feed is watched. The MediaMTX server will close the camera when not used, which is important when depending on battery power.

### Arduino - access to motors/servos
While the integration of the Arduino is straight forward, through Firmata and the Arduino-nodes in Node-RED, the auto-detect feature from the <a href="https://github.com/alexsauter/navalarchimedes/tree/main">main</a> page is still useful.</br></br>
Furthermore, to make startup easier, the available motor are integrated in a selection for Node-RED.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/SubflowArduino.png" width="80%"></p>
The nodes can be downloaded here: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5Arduino.json">RPi5Arduino.json</a>. The specific pins might have to be adjusted to the specific drone.</br></br>
For new ESCs, or when reprogrammed by mistake, one can try to calibrate the ESCs via Node-RED. A first version of this can be found here: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5ArduinoESCCalibration_dev.json">RPi5ArduinoESCCalibration_dev.json</a>. This requires some more understanding, and might have to be done by teachers - and adjusted depending on the type of ESC.</br></br>
In order to measure voltage levels of the battery through the Arduino, one can use a voltage divider circuit to adjust the higher battery voltage to the lower (up to 5V) ADC-levels allowed on the Arduino. However, the potentially high power output of the battery and the issue of using up the battery even in "power off"-mode makes those solutions less safe for users just starting to learn electronics. Access to this option is therefore at the beginner level restricted.

### Telemetry and RC
Both telemetry and radio control (RC) is established via a Pixhawk 4 (tested mostly with firmware version 1.15.4). Communication via mavlink is still enabled through mavlink2rest (using port 8088). In this new version, the control via the RC controller is made more easily available.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/SubflowPixhawk.png" width="80%"></p>
The new subflow can be downloaded here: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5TelemetryRC.json">RPi5TelemetryRC.json</a>.

### Keyboard and Touch support
Through the use of event-listeners as part of e.g. Node-RED dashboard, one can also control the drone through a keyboard or a touch screen. A simple example can be seen here.
<p align="center"><img src="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/figs/SubflowKeyboardTouch.png" width="70%"></p>
The corresponding nodes can be downloaded here: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5KeyboardTouch.json">RPi5KeyboardTouch.json</a>.

### Radar
Radar is still just based on the serial communication, and should work as before. Try nodes her: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5Radar.json">RPi5Radar.json</a>.</br></br>
The radar-chips where earlier placed in the same spot as now the turret is placed at. So future use needs to find a new position.

### RC receiver without Pixhawk
Some might want to use the RC receivers without the need for a Pixhawk. Nodes for that have been started to be developed: <a href="https://raw.githubusercontent.com/alexsauter/navalarchimedes/main/RPi5dev/RPi5X8R_SBUS_dev.json">RPi5X8R_SBUS_dev.json</a>.</br></br>
The goal is the read the SBUS directly at the Raspberry Pi 5. For now, the need for a signal converter fitting to the uart-pins of the Raspberry Pi 5 (pins 8 and 10, only RXD on pin 10 should be needed) is stopping further development. Power levels used will be 3.3V, using pin 1 at 3.3V and pin 9 for GND.

## Authors
* Alexander Sauter