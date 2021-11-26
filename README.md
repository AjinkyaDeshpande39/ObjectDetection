
# 🚗 Vehicle detection, Tracking, Number plate recognition, Speed calculation all on RaspberryPi B4 model. 🍓

In this project, we used model developed by https://github.com/DoubangoTelecom/ultimateALPR-SDK
This repository contains model for Windowns, linux, android and Raspbian OS. The model is very fast and can be used for real time applications with some modifications.
OpenVINO enhances speed of processing, detection hence improves model's performance in moving environment. We will use this model only to perform all tasks. We will also try to interface camera with RaspberryPi to apply this model on real world tasks.



# Table of contents 📑
- [RaspberryPi setup](#raspberrypi-setup)
- [Intregration of Web-Cam](#intregration-of-web-cam)
- [Model](#model)
- [Results](#results)
- [Detection](#detection)
- [In-out counting](#in-out-counting)
- [Tracking and speed calculation](#tracking-and-speed-calculation)
- [Issues](#issues)

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
 
 My code https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/recognizer2.py
 
 You can meke changes to this file but run the recognizer from binaries directory only.

 #Init and deinit only once cause it is time consuming and meaningless to repeat.

 ## Results 😃
 ![](https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/frame.jpg)
 After detection and processing
 ![](https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/frame2.jpg)

 Detailed video can be viewed here - https://youtu.be/Ok_Z6FTORyo
 
 In the images you can see nunmber has been detected and displayed.
 
 Bounding boxes of plate as well as car are displayed
 
 Speed of car at that instance is also mentioned just above the bounding box
 
 count of incoming and outgoing cars is displayed at top left corner
 
 The details of code, methods, analysis, and techniques will be mentioned in next section.

## Detection 🕵️
 
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
 <br> warpedBoxes(bounding boxes) - 
 ```
 {"duration":446,"frame":128,"plates":[{"car":
 {"confidence":66.40625,"warpedBox":[240.1518,331.3265,359.7488,331.3265,359.7488,418.2095,240.1518,418.2095]},
 "confidences":[44.8803,99.60938,87.08838,85.15766,92.15341,87.9593,55.63031,44.88803],"text":"NMMA3*",
 "warpedBox":[271.8652,373.9273,334.6915,373.9273,334.6915,400.7947,271.8652,400.7947]}]}
 ```
 Here structure is as follows - 
 - warpedBox: duration, frame, plates
   - dictionaries related to car info of that plate
     - confidence, warpedBox of car, confidence
   - text over that plate, warpedBox of plate

So, basically a car will be detected only if plate is readable. No plate, no car. Using text as key, car object will be created with these atributes and appended in 'detectedCars' and 'currFrameCars' dictionaries. Usage of these is mentioned in next sectoin.

## In-out counting 👁️‍🗨️
When a car is detected, we create 'car' object for it. At the time of object creation only, we modify the total count of incoming and outgoing count. 
If the centre of car i.e. center o fbounding box is within the red strip specified by us, then count is modified.If centre is in the left half then increment out count. Else, increment in-count. This strip is placed where there is high chance of detecting car. Top left corner is origin (0,0). 
<p align="center">
  <img width="200" src="https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/bb.png">
</p>
Bounding box in the results contain [x1,y1,x2,y2,x3,y3,x4,y4]. Another way could be modify count if bounding box contains line. But reason why i didnt follow this approach is- the number of chances of count modification increases. This is not good. It will become more clear later.

## Tracking, speed calculation 🖲️
The main issue i faced is due to model imperfection or low resolution of image. Suppose one car gets detected with perticular plate "xyz". In next frame, same car gets detected with another plate "xyzw". Regardless of difference in plate number, code will consider it as new entry and calculate speed and count for it. To encounter this pblm, I am keeping track of cars from last frame.\
    Here, IOU intersection over union concept is applied.
    <p align="center">
  <img width="200" src="https://github.com/AjinkyaDeshpande39/ObjectDetection/blob/main/Images/iou.png">
</p>
 <ul> We run a loop for current captured image on lastFrameCars dict. We calculate IOU of bounding boxes of two cars. If IOU is grater than reference value(usually kept greater than 0.5), that means this is the same car. In such case, we just update the attributes and update speed. Else, it means that this is really a different car. So, make new entry. </ul> 


### But how to know which number plate was correct one ? 🤔

 In the results, we obtain confidence score for number plates. We will update the number plate based on this we can update the number plate.
 
 This is how tracking of car is done in simple manner.
    
 
 Since i didnt know the actual ground measurements, i am considering 
 
 *speed  =  (change in y co-ordinate)/(frames passed)*
 
 If we have actual measurements, we can convert this further to real dimensions. Speed is updated for every next appearance of that car.

## Issues ⁉️
- The processing time for this model is very large. I was getting near to 1fps video output at 1280x720fps on RPi4B
- The bounding boxes are varying for every frame per car. This generates some illusion while watching the video.
- Small number plates are removed. (dont know why)
 
