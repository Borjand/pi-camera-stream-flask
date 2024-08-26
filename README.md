# Make you own Raspberry Pi Camera Stream

Create your own live stream from a Raspberry Pi using the Pi camera module. Build your own applications from here.

## How it works
The Pi streams the output of the camera module over the web via Flask. Devices connected to the same network would be able to access the camera stream via

```
<raspberry_pi_ip:5000>
```

Since raspbian includes the avahi-daemon linux artifacts, it is also possible to connect using the hostname of the raspberry pi (which can be modified as preferred to make the connection easier):

```
http://raspberrypi.local:5000
```

## Screenshots
| ![Setup](readme/pi-stream-client.jpg) | ![Live Pi Camera Stream](readme/pi-stream-screen-capture.jpg) |
| ------------------------------------- | ------------------------------------------------------------- |
| Pi Setup                              | Pi - Live Stream                                              |

## Preconditions

* Raspberry Pi 4, 2GB is recommended for optimal performance. However you can use a Pi 3 or older, you may see a increase in latency.
* Raspberry Pi 4 Camera Module or Pi HQ Camera Module (Newer version)
* Python 3 recommended.
* Raspbian Buster 32 bits (full Desktop version) as Operating System

## Library dependencies
Install the following dependencies to create camera stream.

```
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install libatlas-base-dev
sudo apt-get install libjasper-dev
sudo apt-get install libqtgui4
sudo apt-get install libqt4-test
sudo apt-get install libhdf5-dev

sudo pip3 install flask
sudo pip3 install numpy==1.21.6
sudo pip3 install opencv-contrib-python==4.5.4.60
sudo pip3 install imutils
sudo pip3 install opencv-python==4.5.4.60

```

Note: This installation of opencv may take a while depending on your pi model.

OpenCV alternate installation (in the event of failed opencv builds):

```
sudo apt-get install libopencv-dev python3-opencv
```

## Step 1 – Cloning Raspberry Pi Camera Stream
Open up terminal and clone the Camera Stream repo:

```
cd /home/pi
git clone https://github.com/Borjand/pi-camera-stream-flask.git
```

## Step 2 – Launch Web Stream

Note: Creating an Autostart of the main.py script is recommended to keep the stream running on bootup.
```bash cd modules
sudo python3 /home/pi/pi-camera-stream-flask/main.py
```

## Step 3 – Autostart your Pi Stream

Optional: A good idea is to make the the camera stream auto start at bootup of your pi using a systemd Linux service. You will now not need to re-run the script every time you want to create the stream. You can do this by going editing the /etc/profile to:

```
sudo nano /etc/systemd/system/pi-camera-stream-flask.service
```

Add the following:

```
[Unit]
Description=Pi Camera Stream Flask Service
After=network.target

[Service]
ExecStart=/usr/bin/python3 /home/pi/pi-camera-stream-flask/main.py
WorkingDirectory=/home/pi/pi-camera-stream-flask
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
```

After this, reload **systemd** to led it to detect the new service, and enable the service to be started at bootup

```
sudo systemctl daemon-reload
sudo systemctl enable pi-camera-stream-flask.service
```

Finally, you can immediately verify the functioning of the service with the following commands:

```
sudo systemctl start pi-camera-stream-flask.service
sudo systemctl status pi-camera-stream-flask.service
```

This would cause the following terminal command to auto-start each time the Raspberry Pi boots up. This in effect creates a headless setup - which would be accessed via SSH.
Note: make sure SSH is enabled.

## More Projects:

This project is a slightly customized version of the Pi Camera project build on [smartbuilds.io](https://smartbuilds.io).

As an additional resource, you can find an illustrative video of how the assembly and the installation is done in [YouTube](https://www.youtube.com/watch?v=zfBHD4v8hD0&t=636s).

