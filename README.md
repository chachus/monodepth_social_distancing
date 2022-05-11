# Monodepth Social Distancing Monitoring

<!-- [![code style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
![Python version](https://img.shields.io/badge/python-python%203.9-brightgreen) -->

## Introduction

This is the repository for our [paper](https://arxiv.org/abs/2204.01693) *Monitoring social distancing with single image depth estimation*. 
Using monodepth neural networks, particular CNNs that learn to perform single image depth estimation, we are able to monitor social distancing in real time using a single camera setup.

<!-- It combines a set of tools to provide different information on the state of social distancing:
- Monodepth estimation: provided by MiDaS [https://github.com/isl-org/MiDaS]
- People Instance Segmentation: provided using Detectron2 by Facebook Research [https://github.com/facebookresearch/detectron2]
- People Traking: provided by DEEP SORT algorithm [https://github.com/nwojke/deep_sort] -->

As is monodepth estimation doesn't give any metric information of the estimated depth. To do so a **scaling operation** is needed. In our case a smartphone app powered by Google ARCore framework is used to obtain a sparse pointcloud of known dephts.

### ARCore pointcloud
To register the 3D points acquired from the phone to the camera 3D reference system, a trasformation is computed using known points on the scene or a known pattern (a chessboard in this case). 
![image (1)](https://user-images.githubusercontent.com/45073899/138907701-89e03c48-c8be-435f-95f8-ac0effc845e3.jpg)

### Output
This is an example of the output:
![0000000189](https://user-images.githubusercontent.com/45073899/138482201-15120da0-1c5e-4b24-b539-301e0cda6bea.jpg) 
On the image are drawn the distances between each person in the scene and the respective segmentation mask. The overlay is color coded by the higher risk zone for that person:
- `red` if the there is a distance < 1 meter
- `yellow` if there is a distance between 1 and 2 meters
- `green` if there is a distance > 2 meter

### Dataset
The dataset on which the tests were performed will be made available soon.
