# WIRC - Infrared camera basics

This repository contains information that may be useful in understanding how infrared light and Raspberry Pi cameras can be used to monitor bats.
There are two other repositories on the same topic, one containing a camera server application for the Raspberry Pi and another containing a client application that can be used on other platforms to control and use the camera server.

- https://github.com/cloudedbats/wirc_2025_backend
- https://github.com/cloudedbats/wirc_2025

## Background

It's not easy to study and monitor bats. We can't hear them and we can't see them when they are active without tools. There are tools like heterodyne and ultrasound recording devices that are widely used, but there aren't many tools available if we want to watch them to find out what they're doing in the dark of night. In my opinion, sound alone is not enough, but if we can combine audio recordings with video, we should be able to get more confident species identifications and also learn more about the behavior of these amazing animals.

I have been working for some years on a DIY detector based on the Raspberry Pi computers in combination with high quality ultrasonic microphones. Now it is time for the next step and add a camera system for night vision as a complement to the existing system for ultrasound.

## About infrared cameras

It's not a good idea to expose bats to bright visible light when recording videos, and in some countries you're not allowed to do so, but we can use infrared light instead.

The wavelength range of light that we can see is from about 400 nm to 700 nm. The wavelength of infrared light that we use as "invisible light" is usually around 850 nm and sometimes around 950 nm. Camera sensors are more sensitive to the 850 nm version but the light sources emit a small amount of visible red light. 950 nm is mainly used in surveillance applications where visible red light cannot be accepted.

**Thermal imaging cameras**, which use infrared light with very long wavelength up to 14000 nm, can be used to detect animals that emit heat. We then don't need a light source ourselves, but the image resolution is usually low and it's not possible to see details, especially on fast-flying bats.

**Surveillance cameras** with night vision can be used, but they are not designed to capture sharp images of fast-moving objects, especially when light is a limiting factor. This type of cameras often have problems with "rolling shutter", which is an effect of the sensor being read line by line and it takes a certain amount of time for the entire sensor to be read.

**Common camera sensors** for photos and video have a sensor that captures blue, green and red light. Then there is also an IR-cut filter that is used to remove the red light that is outside the range that we can see. The blue and green parts of the sensor can't be used if we only use infrared light as a light source.

**Cameras with monochrome sensors** can only record grayscale images. They can be used with or without filters such as IR-cut, IR-pass, or color filters.
When it is dark and the only light source is the infrared light that we use, then an IR-pass filter or using a clean path with no filters at all will work.

Note: It can be confusing when things are marker as "IR". An IR-filter can be either an IR-cut or IR-pass filter. If the camera contains an IR-cut filter it has to be removed before we can use them in combination with infrared light sources.

## Sharp images of fast moving objects with visible light

There are numerous of incredible sharp photos of flying bats in books and on the internet. They mostly rely on the short pulses of light that camera flashes can produce. The exposure time of the camera can be much longer since we are out when it's dark. This is what you need:
- A standard system camera, both DSLR and mirrorless cameras can be used.
- A number of camera flashes that can be synchronized exactly. 
- Cooperative bats that are flying where you want them to fly. This is the most tricky part.
- Access to a "depth-of-field calculator".
- A tripod if you want a sharp background/surroundings.

This is a typical setup for a full frame camera if you want a picture including both the flying bat near the camera and the further away background. 
- Set both the camera and the flashes in manual mode.
- Try to find a setup for depth-of-field to go from 0.5 m to infinity. For example focal length 18 mm, aperture f/13, and a focal distance 1 m will do that on a full frame camera.
- Then adjust the cameras exposure time to capture the faint light from the background, for example 60 sec.
- To freeze the movements of a flying bat the flashes should be set to manual mode to at least M 1/16, which is comparable to an exposure time of about 1/10000 sec. Try M 1/32 or M 1/64 if needed to get a sharper image, but then you need more light.

To take a photo of the bat only you don't need a depth-of-field that wide and can use another aperture and focal length. A tripod is then not needed due to the extremely short exposure time. Camera movement is not a problem in this case, try to keep the effects of the moving object to a minimum. 

## Sharp images/videos of fast moving objects with infrared light

Here we have two possibilities. Either mimic the camera flashes with infrared light sources that also can emit short light pulses, or use light sources with permanent infrared light and then a camera sensor that can handle short exposure time.

There are many infrared light sources, mainly used for surveillance application, that can be used. Just search for  "LED Flood Light IR Night Vision" on the internet.

The camera module I use mostly for test and development is called OV9281. It is not a standard Raspberry Pi camera but it is supported by the Raspberry Pi team. The OV9281 camera module is a 1 MP monochrome camera that can capture videos where each frame is exposed down to 0.029 ms, which is shorter than 1/34000 sec. OV9281 is also a "global shutter" camera that eliminates all problems with "rolling shutter". 

Another camera with global shutter that I use is the "Raspberry Pi Global Shutter Camera" that features a 1.6 MP Sony IMX296 color sensor.  

Part of the specification for OV9281:
Shutter type: Global Shutter, 
Color: Monochrome, 
Resolution: 1280x800, 
Pixel size: 3.0 µm.
Sensor image area: 3.896 x 2.453 mm.

The sensor image area for a full frame camera is 36.0 × 24.0 mm. That means that a 8 mm lens on the OV9282 camera will be comparable to a 72 mm lens on a full frame camera regarding field of view.

## The Raspberry Pi camera system

More information is available here:

- https://www.raspberrypi.com/documentation/accessories/camera.html
- https://datasheets.raspberrypi.com/camera/picamera2-manual.pdf
- https://www.raspberrypi.com/documentation/computers/camera_software.html

## Installation

This is a short instruction on how to install camera support on a Raspberry Pi computer. 

The first step is to use the **Raspberry Pi Imager** to install the *Raspbian Pi OS* on a SD card.
I you want to also install the WURB software for sound then it is handy to use the same user name here.

Use these settings, or similar, when running the **Raspberry Pi Imager**:

- Select OS version: Raspberry Pi OS Lite (32-bit). 
- Hostname: wurb01c
- User: wurb
- Password: your-secret-password
- WiFi SSID: your-home-network
- Password: your-home-network-password
- Wireless LAN country: your-country-code
- Time zone: your-time-zone
- Activate SSH. Is located under the tab "Services".

Then connect to the Raspberry Pi with SSH and do an update.

    ssh wurb@wurb01.local
    
    sudo apt update
    sudo apt upgrade -y

Then install some linus packages that is common for both WURB (my system for sound) and WIRC (the new one for video/images). 

    sudo apt install git python3-venv python3-dev -y
    sudoapt install  libatlas-base-dev libopenblas-dev -y
    sudoapt install pulseaudio pmount -y

Additions for camera support.

    sudo apt install -y python3-picamera2
    sudo apt install -y python3-pyqt5 python3-opengl
    sudo apt install -y python3-prctl python3-kms++ 
    sudo apt install -y ffmpeg
    sudo apt autoremove

Then install the software in this repository if you want to run the example code.

    git clone https://github.com/cloudedbats/wirc_2025_backend.git

Then attached cameras should be available after a reboot if they are directly supported by Raspberry Pi.

If you use other cameras like the OV9281 you have to tell the system what you are using.

    sudo nano /boot/firmware/config.txt

    # Replace "camera-auto-detect=1" with
    camera-auto-detect=0
    
    # At the end of the file add this:
    dtoverlay=ov9281

    # If you are using Raspberry Pi 5 and want to connect two cameras you have to add
    # it like this (with two global shutter cameras as an example):
    dtoverlay=ov9281,cam0
    dtoverlay=imx296,cam1

## Example code

TODO...to be continued...
