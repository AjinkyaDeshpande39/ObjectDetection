
# 🚗 Vehicle detection, Tracking, Number plate recognition, Speed calculation all on RaspberryPi B4 model. 🍓

In this project, we used model developed by https://github.com/DoubangoTelecom/ultimateALPR-SDK
This repository contains model for Windowns, linux, android and Raspbian OS. The model is very fast and can be used for real time applications with some modifications.
OpenVINO enhances speed of processing, detection hence improves model's performance in moving environment. 


# Raspberry_Pi_NPD
Implementation of Number plate  detection algorithm  on raspberry pi

# Table of contents
- [Intregration of Web-Cam](#intregration-of-web-cam)
- 



## Intregration of Web-Cam

- Here, the ip-webcam method is used to connect the web cam to the R-Pi. 
- The feed of the mobile camera is hosted on a local IP address using [IP Webcam](https://play.google.com/store/apps/details?id=com.pas.webcam&hl=en_IN&gl=US)
- The R-pi is linked to the mobile phone using wifi-hostpot, so they both have a local address. 
- We are now receiving the footage from the mobile camera at the R-pi's local address.
- This video was fetched in Python with the help of the url library. 
