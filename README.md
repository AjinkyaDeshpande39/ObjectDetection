
# 🚗 Vehicle detection, Tracking, Number plate recognition, Speed calculation all on RaspberryPi B4 model. 🍓

In this project, we used model developed by https://github.com/DoubangoTelecom/ultimateALPR-SDK
This repository contains model for Windowns, linux, android and Raspbian OS. The model is very fast and can be used for real time applications with some modifications.
OpenVINO enhances speed of processing, detection hence improves model's performance in moving environment. We will use this model only to perform all tasks. We will also try to interface camera with RaspberryPi to apply this model on real world tasks.



# Table of contents
- [RaspberryPi setup](#raspberrypi-setup)
- [Intregration of Web-Cam](#intregration-of-web-cam)
- 

## RaspberryPi setup 💻
The model which i am using is RaspberryPi 4B it has 4GB RAM.
If you are new to raspberryPi and have not even flashed OS, or RPi is ready but packages are not installed, do follow this blog -: https://www.pyimagesearch.com/2019/09/16/install-opencv-4-on-raspberry-pi-4-and-raspbian-buster/
This shows complete setup from scratch. Setup includes flashing OS, configuring RPi and installing imp packages of python. Read each and every section of this blog. You may scip some as per the configurations of prebuild libraries or as per your requirements. OpenCV will be required in this project.

You can also install RPi OS using RaspberryPi imager. https://www.youtube.com/watch?v=ntaXWS8Lk34 checkout this video 
*This method requires good internet connectivity throughout the process of downloading and flashing OS packages by packages for more than 1.2GB
I will suggest to have zip file downloaded. Then you can flash it any number of times. as suggested in first method.*

Apart from this, chekck raspberrypi's official website. https://www.raspberrypi.com/  . Here you can find some usefull updates, documentation, FAQ. 

Once you install OS, you have to connect RPi to a monitor/TV through HDMI cable for first boot. There RPi partiotions memory, set password, sets video resolution, config interfacing servers etc. This process is imp.

After that, you can either operate RPi on that monitor itself or you can access it through your laptop using SSH and VNC.
For such remote access, you can either refer to official website, or refer this video https://youtu.be/uLwj4Wj7pRI.
I will recommend to connect RPi to your laptop's hotspot so that you can directly see the IP address from hotspot settings.

### Some tips and errors:
- Use python3
- Install by using command : sudo pip3 install \<package\>
- Sometimes this may not work. Then try : sudo apt-get install python-\<package\>
 or sudo apt-get install \<package\>

- Imprt error PIL :
- - sudo python3 -m pip install wheel
- - sudo python3 -m pip install pandas

- workon cv : Import error PIL
- - sudo apt-get install python-pillow
- - (or) sudo pip3 install pillow.
- Installing pandas:
- - sudo python3 -m pip install wheel
- - sudo python3 -m pip install pandas

- VNC viewer cant show desktop image: change video resolution.
## Intregration of Web-Cam

- Here, I used IPWebcam to connect the web cam to the RPi. 
- The feed of the mobile camera is hosted on a local IP address using [IP Webcam](https://play.google.com/store/apps/details?id=com.pas.webcam&hl=en_IN&gl=US)
- The RPi is linked to the mobile phone using wifi-hostpot, so they both have a local address. 
- We are now receiving the footage from the mobile camera at the R-pi's local address.
- This video was fetched in Python with the help of the url and cv2 library.
By this method, you can connect high resolution mobile camera. But since it is LAN connection, fetching frame is slow and laggs.  
