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

![](https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/blender.png)

### Summary of Development Flow

The hardware used is the Xilinx Kria KV260, the camera kit for accelerated computer vision processing, and Ethernet connectivity. However,due to limited spare time in my last semester I didn't realize using the DPU of KV260 to accelerate the MocapNET which means I just use the default C API of tensorflow of the MocapNET.So although the MocapNET can run on the KV260 up to real time now but it can be more faster if I use the DPU of KV260.Anyway,I will the VART library to reconstruct the code of the model inference in the future but there is not enough time for me to finish the plan befor the deadline of the contest.

### PC: Setup SD Card Image

First we need to prepare the SD card for the Kria KV260 Vision AI Starter Kit.

We will download using Ubuntu 20.04.3 LTS. Download the image from the website and save it on your computer.

- https://ubuntu.com/download/xilinx

![https://github.com/Hezhexi2002/KV260-Mocap/blob/main/assets/ubuntu.png]()

On your PC, download the Balena Etcher to write it to your SD card.

- https://www.balena.io/etcher/

