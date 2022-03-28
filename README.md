# KV260-Mocap
## Things used in this project

### Hardware components

------

 [AMD-Xilinx Kria KV260 Vision AI Starter Kit](https://www.hackster.io/xilinx/products/kria-kv260-vision-ai-starter-kit?ref=project-f9dad2)                                                                                                                                                       x1

HIKVISION E14a 2K usb camera                                                                                                                                                                           x1

### Software apps and online services

------

 [AMD-Xilinx Vitis Unified Software Platform](https://www.hackster.io/xilinx/products/vitis-unified-software-platform?ref=project-f9dad2)

 [Snappy Ubuntu Core](https://www.hackster.io/Ubuntu/products/snappy-ubuntu-core?ref=project-f9dad2)

## Story

### Introduction

**Issue:**  The traditional and most used mocap are based on the complex optical system,however,there are huge demands for a portable and affordable mocap device in many different kind of industries such as animation manufacturing,healthcare industry and so on.In that case,I presented a mocap using the opensouce framework MocapNET which can estimate the 3D human pose directly in the popular [Bio Vision Hierarchy (BVH)](https://en.wikipedia.org/wiki/Biovision_Hierarchy) format, given estimations of the 2D body joints originating from monocular color images.

**Goal:**  My project aims to use KV260 running the MocapNET to make it possible for anyone to achieve motion capture anywhere without the bulky equipments requierd by the traditional optical mocap system.Bsides,the KV260-Mocap can output the BVH files and then edit in Blender,and also thanks to the high computing capabilities of KV260,the KV260-Mocap can basically achieve the motion capture in real time.When running the MocapNET,the animator can view the human model directly and adjust the parameters in real time which can accelerate their workflow a lot.For sure,the doctors can use it to build a custom skeleton model for their patients which help them to locate the lesions point of the patients too.

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/example.jpeg)

### Summary of Development Flow

The hardware used is the Xilinx Kria KV260, the camera kit for accelerated computer vision processing, and Ethernet connectivity. I choose the HIKVISION E14a 2K usb camera to capture frame in a high resolution and then I will use the MocapNET to achieve the motion capture in real time and generate the BVH files which can be edited in 3D editing and animation software such as [Blender](https://www.blender.org/) .

### PC: Setup SD Card Image

First we need to prepare the SD card for the Kria KV260 Vision AI Starter Kit.

We will download using Ubuntu 20.04.3 LTS. Download the image from the website and save it on your computer.

- https://ubuntu.com/download/xilinx

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/ubuntu.png)

On your PC, download the Balena Etcher to write it to your SD card.

- https://www.balena.io/etcher/

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/etcher.png)

Once done, your SD card is ready and you can insert it into your Kria.

### Kria: Setup Xilinx Ubuntu

Connect a USB Keyboard, USB Mouse, USB Camera, HDMI/DisplayPort and Ethernet to the Kria.

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/device.jpg)

Connect the power supply to turn on the Kria and you will see the Ubuntu login screen.

> *The default login credentials are:****username\***: ubuntu****password\***: ubuntu*

But then as soon as I typed the default password, **ubuntu**, on this first boot it immediately forced me to change it to set my own password.

Next, update the system to the latest by doing system updates and calling this command

```
sudo apt upgrade
```

Now you have prepared everything already for the next step.

### Kria: Install the MocapNET

#### Building the library

First of all you need to install the dependencies before building the library of MocapNET with following commands

```
sudo apt-get install git build-essential cmake libopencv-dev libjpeg-dev libpng-dev libglew-dev libpthread-stubs0-dev
```



Then you can clone the MocapNET repository and run the following command to install it

```
git clone https://github.com/FORTH-ModelBasedTracker/MocapNET

cd MocapNET

./initialize.sh
```

After performing changes to the source code, you do not need to rerun the initialization script. You can recompile the code by using :

```
cd build 
cmake .. 
make 
cd ..
```

However,you have to deal with a error when you are building the whole project at last

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/libtensorflow_error.png)

That is because the initialize.sh will install the tensorflow library so for x86_64 architecture device by default while the cpu of KV260 belongs to arm64 architecture.So I found a precompiled libtensorflow.so for jetson nano which also works for KV260 from this link:[libtensorflow.tar.gz](https://github.com/nevillerichards/tensorflow/releases)

After you download the libtensorflow.tar.gz from the url and extract the file you will see a include folder and a lib folder like this

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/precompiled_libtensorflow.png)

And all you need to do is to run following command to replace the original lib folder of the default libtensorflow in `~/Desktop/MocapNET/dependencies/libtensorflow/lib`  

```
# ~/Downloads is where I extract the libtensorflow.tar.gz to,you can change the directory depend on your path where the libtensorflow.tar.gz is extracted to.
cd ~/Downloads

sudo cp lib ~/Desktop/MocapNET/dependencies/libtensorflow
```

Finally when you re-run the initialize.sh you will successfully build the MocapNET like this

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/success_build.png)

And yep you have successfully installed MocapNET on KV260 and let's begin to have fun with it!

#### Updating the library

The MocapNET library is under active development, the same thing is true for its dependencies.

In order to update all the relevant parts of the code you can use the [update.sh](https://github.com/FORTH-ModelBasedTracker/MocapNET/blob/master/update.sh) script provided.

```
./update.sh
```

If you made changes to the source code that you want to discard and want to revert to the master you can also use the [revert.sh](https://github.com/FORTH-ModelBasedTracker/MocapNET/blob/master/revert.sh) script provided

```
./revert.sh
```

### Kria: Test the MocapNET 

#### Testing the library and performing benchmarks

To test your OpenCV installation as well as support of your webcam issue :

```
./OpenCVTest --from /dev/video0 
```

To test OpenCV support of your video files issue :

```
./OpenCVTest --from /path/to/yourfile.mp4
```

#### Live Demo

Assuming that the OpenCVTest executable described previously is working correctly with your input source, to do a live test of the MocapNET library using a webcam issue :

```
./MocapNET2LiveWebcamDemo --from /dev/video0 --live
```

To dump 5000 frames from the webcam to out.bvh instead of the live directive issue :

```
./MocapNET2LiveWebcamDemo --from /dev/video0 --frames 5000
```

To control the resolution of your webcam you can use the --size width height parameter, make sure that the resolution you provide is supported by your webcam model. You can use the v4l2-ctl tool by executing it and examining your supported sensor sizes and rates. By issuing --forth you can use our FORTH developed 2D joint estimator that performs faster but offers lower accuracy

```
 v4l2-ctl --list-formats-ext
./MocapNET2LiveWebcamDemo --from /dev/video0 --live --forth --size 800 600
```

Testing the library using a pre-recorded video file (i.e. not live input) means you can use a slower but more precise 2D Joint estimation algorithm like the included OpenPose implementation. You should keep in mind that [this OpenPose implementation](https://github.com/FORTH-ModelBasedTracker/MocapNET/blob/master/src/MocapNET1/MocapNETLiveWebcamDemo/utilities.cpp#L213) does not use PAFs and so it is still not as precise as the official OpenPose implementation. To run the demo with a prerecorded file issue :

```
./MocapNET2LiveWebcamDemo --from /path/to/yourfile.mp4 --openpose
```

BVH output files are stored to the "out.bvh" file by default. If you want them to be stored in a different path use the -o option. They can be easily viewed using a variety of compatible applicatons. 

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/live_demo.png)

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/out_bvh.png)

This first picture shows the result of the live demo and second shows the BVH file generated by MocapNET in Blender,and you can find more usages in the [README.md](https://github.com/FORTH-ModelBasedTracker/MocapNET/blob/master/README.md) of MocapNET.

### Conclusion

As you can see the result of the MocapNET running on KV260 is not so good due to the limited spare time in my last semester and I didn't realize using the DPU of KV260 to accelerate the MocapNET which means I just use the Cortex A53 processor of KV260 to run the MocapNET.So although the MocapNET can run on the KV260 up to real time now but it can be more faster if I use the DPU of KV260.Anyway,I plan to use the VART library to reconstruct the code of the model inference in the future but there is not enough time for me to finish the plan befor the deadline of the contest.I believe it will be more faster than now with the speed up of DPU.
