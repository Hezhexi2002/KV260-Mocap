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

![image-20220327095405875](C:\Users\12235\AppData\Roaming\Typora\typora-user-images\image-20220327095405875.png)

**Goal:**  My project aims to use KV260 running the MocapNET to make it possible for anyone to achieve motion capture anywhere without the bulky equipments requierd by the traditional optical mocap system.Bsides,the KV260-Mocap can output the BVH files and then edit in Blender,and also thanks to the high computing capabilities of KV260,the KV260-Mocap can basically achieve the motion capture in real time.When running the MocapNET,the animator can view the human model directly and adjust the parameters in real time which can accelerate their workflow a lot.For sure,the doctors can use it to build a custom skeleton model for their patients which help them to locate the lesions point of the patients too.

