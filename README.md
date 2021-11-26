
# 🚗 Vehicle detection, Tracking, Number plate recognition, Speed calculation all on RaspberryPi B4 model. 🍓

In this project, we used model developed by https://github.com/DoubangoTelecom/ultimateALPR-SDK
This repository contains model for Windowns, linux, android and Raspbian OS. The model is very fast and can be used for real time applications with some modifications.
OpenVINO enhances speed of processing, detection hence improves model's performance in moving environment. We will use this model only to perform all tasks. We will also try to interface camera with RaspberryPi to apply this model on real world tasks.



# Table of contents
- [RaspberryPi setup](#raspberrypi-setup)
- [Intregration of Web-Cam](#intregration-of-web-cam)
- [Model](#model)
- [Results](#results)
- [Detection](#detection)

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
```
sudo python3 -m pip install wheel
sudo python3 -m pip install pandas
```
- workon cv : Import error PIL
```
sudo apt-get install python-pillow
(or) 
sudo pip3 install pillow.
```
- Installing pandas:
```
sudo python3 -m pip install wheel
sudo python3 -m pip install pandas
```
- VNC viewer cant show desktop image: change video resolution.
## Intregration of Web-Cam 📹

- Here, I used IPWebcam to connect the web cam to the RPi. 
- The feed of the mobile camera is hosted on a local IP address using [IP Webcam](https://play.google.com/store/apps/details?id=com.pas.webcam&hl=en_IN&gl=US)
- The RPi is linked to the mobile phone using wifi-hostpot, so they both have a local address. 
- We are now receiving the footage from the mobile camera at the R-pi's local address.
- This video was fetched in Python with the help of the url and cv2 library.
By this method, you can connect high resolution mobile camera. But since it is LAN connection, fetching frame(fps) is slow and laggs.  
![](https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/Hnet-image.gif)


## Model
clone this repo https://github.com/DoubangoTelecom/ultimateALPR-SDK. Download it in RPi.

You can follow this blog to learn how to clone repo https://geektechstuff.com/2019/09/09/introduction-to-github-raspberry-pi/
This repo is large. 2GB. make sure you have uninterrupted internet thoughout process.

After downloading complete, follow the instructions about setting up benchmark, building apk, building python extension, installing tensorflow.
All the instructions are given in README.md files. If you read the repo line by line, you could do it by yourself. The most common issues you could face during this process are also given at bottom. 

Some key points ill mention here -
- DoubangoTelecom claims that at max it works with 12fps on RPi. This sdk is open source and doent require licence
- This is a command line application. A sample file is given in samples/python/recognizer folder.
- Before running this file, you have to make sure some dependencies are installed
- The SDK is developed in C++11 and you'll need glibc 2.27+ on Linux and Visual C++ Redistributable for Visual Studio 2015 (any later version is ok) on Windows. You most likely already have these dependencies on you machine as almost every program require it.
- I didnt use OpenVINO. It accelerates the process by a large amount. Although OpenvVINO is for windows, it can be used in raspbian/debian os using Intel's NeuralComputeStick. It is basically a VPU.
- To check missing dependencies : Linux x86_64: Use ldd <your-shared-lib> on binaries/linux/x86_64/libultimate_alpr-sdk.so 
- You must build python extension : https://github.com/DoubangoTelecom/ultimateALPR-SDK/blob/master/python/README.md clearly instructed here.
- install libtensorflow.so https://github.com/DoubangoTelecom/ultimateALPR-SDK/blob/master/samples/c++/README.md#gpu-acceleration-tensorflow-linux. SDK uses tensorflow acceleration for fast computing.
 - Before trying to run sample file, you have to move to cd ultimateALPR-SDK/binaries/raspbian/armv7l
 - then try - 
```
 PYTHONPATH=$PYTHONPATH:.:../../../python \
LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH \
python ../../../samples/python/recognizer/recognizer.py --image ../../../assets/images/lic_us_1280x720.jpg --assets ../../../assets
```
 
 My code - https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/recognizer2.py
 
 You can meke changes to this file but run the recognizer from binaries directory only.

 ## Results
 ![](https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/frame.jpg)
 After detection and processing
 ![](https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/frame2.jpg)

 Detailed video can be viewed here - https://youtu.be/Ok_Z6FTORyo
 
 In the images you can see nunmber has been detected and displayed.
 
 Bounding boxes of plate as well as car are displayed
 
 Speed of car at that instance is also mentioned just above the bounding box
 
 count of incoming and outgoing cars is displayed at top left corner
 
 The details of code, methods, analysis, and techniques will be mentioned in next section.

## Detection
 The piece of code 
 ```
 warpedBox,texts_lst = checkResult("Process",
                ultimateAlprSdk.UltAlprSdkEngine_process(
                    format,
                    image.tobytes(), # type(x) == bytes
                    width,
                    height,
                    0, # stride
                    exifOrientation
                    )
        )
    return(warpedBox,texts_lst)
 ```
 Calls pretrained model which is basically a version of YOLO. Runs our frame through that NN and generated some results. Results contain -
 <br> warpedBoxes(bounding boxes) of plates, their confidance score and 
