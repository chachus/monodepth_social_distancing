# Monodepth Social Distance

[![code style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
![Python version](https://img.shields.io/badge/python-python%203.9-brightgreen)

## Introduction

This repository contains the code for *Monitoring social distancing with single image depth estimation*. Using monodepth neural networks, particular CNNs that learn to perform single image depth estimation, we are able to monitor social distancing using a single camera setup.

It combines a set of tools to provide different information on the state of social distancing:
- Monodepth estimation: provided by MiDaS [https://github.com/isl-org/MiDaS]
- People Instance Segmentation: provided using Detectron2 by Facebook Research [https://github.com/facebookresearch/detectron2]
- People Traking: provided by DEEP SORT algorithm [https://github.com/nwojke/deep_sort]

As is Monodepth estimation doesn't give any metric information of the estimated depth. To do so a **scaling operation** is needed. In our case a smartphone app powered by Google ARCore framework is used to obtain a sparse pointcloud of known dephts.

## Dependecies
The code has been tested using Python 3.9. All other dependencies are indicated into [requirements.txt](requirements.txt)

## Configuration
The [configuration](configurations/config.yaml) file can be edited to change paths to necessary files, the models used and some behaviours.

## Running the demo
The following example shows how to execute the demo on a series of images already available.

### ARCore pointcloud
As staded previously, ARCore is used to acquire a pointcloud of known depths. To register the 3D points acquired from the phone to the camera 3D reference system, a trasformation is computed using known points on the scene or a known pattern (a chessboard in this case). 
![image (1)](https://user-images.githubusercontent.com/45073899/138907701-89e03c48-c8be-435f-95f8-ac0effc845e3.jpg)
To compute and save the transformation, run the script `acquire_arcore.py`:
```
python acquire_arcore.py 
```
***Before running the script, in the configuration file modify the varius paths pointing to the ARCore to Camera calibration phase***

Once the transformation is computed, run the demo like:
```
python demo.py \
    --sequence \
    <path to images> <images format> \
    --[video|novideo]
```
- With option `--sequence` a sequence of images is used as input. Flag must be followed by the images path and their format.
- To use it with a live camera change `--sequence` with `--camera`. If there are different cameras available, follow the flag with the index.
- Use `-h` to print help on usage, `--helpfull` to print all the flags available.
- The option `video` shows a window with the output of the program and saves a video of the output sequence.

### Monodepth & Segmentation models
The program uses the most accurate models available for Monodepth estimation and People Instance segmentation, which are also the more computationally heavy.

### Output
This is an example of the output:
![0000000189](https://user-images.githubusercontent.com/45073899/138482201-15120da0-1c5e-4b24-b539-301e0cda6bea.jpg) 
On the image are drawn the distances between each person in the scene and the respective segmentation mask. The overlay is color coded by the higher risk zone for that person:
- `red` if the there is a distance < 1 meter
- `yellow` if there is a distance between 1 and 2 meters
- `green` if there is a distance > 2 meter
