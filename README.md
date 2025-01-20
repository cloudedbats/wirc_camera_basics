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

*Thermal imaging cameras*, which use very long wavelength infrared light, can be used to detect animals that emit heat. We don't need a light source ourselves, but the image resolution is usually low and it's not possible to see details, especially on fast-flying bats.

*Surveillance cameras* with night vision can be used, but they are not designed to capture sharp images of fast-moving objects, especially when light is a limiting factor.

*Common cameras* for photos and video have a sensor that captures blue, green and red light. Then there is also an IR-cut filter that is used to remove the red light that is outside the range that we can see. The blue and green parts of the sensor can't be used if we only use infrared light as a light source.

*Cameras with monochrome sensors* can only record grayscale images. They can be used with or without filters such as IR-cut, IR-pass, or color filters.
When it is dark and the only light source is using infrared light, then no filter or IR-pass filter will work.

Note: It can be confusing when things are marker as "IR". An IR-filter can be either an IR-cut or IR-pass filter. If the camera contains an IR-cut filter it has to be removed.

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

To take a photo of the bat only you don't need the depth-of-field that wide and can use another aperture and focal length. A tripod is then not needed due to the extremely short exposure time. Camera movement is not a problem in this case, try to keep the effects of the moving object to a minimum. 

## Sharp images/videos of fast moving objects with infrared light

Here we have two possibilities. Either mimic the camera flashes with infrared light sources that also can emit short light pulses, or use light sources with permanent infrared light and then camera sensors that can handle short exposure time.

There are many light sources, mainly used for surveillance application, that can be used. Just search for  "LED Flood Light IR Night Vision" on the internet.

The camera module I use mostly for test and development is called "OM9281". It is not a standard Raspberry Pi camera but it is supported by the Raspberry Pi team.
The "OM9281" camera module is a 1 MP monochrome camera that can capture videos where each frame is exposed down to 0.029 ms, which is shorter than 1/33000 sec. It is also a "global shutter" camera that eliminates all problems with "rolling shutter" that is common when capturing fast moving things.

Part of the specification for "OM9281":
Model: OV9281, 
Color: Monochrome, 
Shutter type: Global Shutter, 
Resolution: 1280x800, 
Sensor image area: 3.896 x 2.453 mm (Full frame is 36×24 mm),
Pixel size: 3.0 µm.


TODO...to be continued...
