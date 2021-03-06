# kitti_dogma_dataset

## Introduction
This dataset contains dynamic occupancy grid (DOGMa) estimations for the KITTI 3D object detection dataset. It was obtained by running the CMCDOT tracking algorithm (https://hal.archives-ouvertes.fr/hal-01205298/) on the KITTI dataset raw sequences and then selecting the samples contained in the 3D object detection dataset.

The dataset is hosted at: https://archive.org/details/kitti_dogma_dataset  
As stated in the hosting site (Internet Archive), the usage license is CC BY-NC-SA 4.0.

<img src="/multimedia/state_grids.gif"/>

## Data description
We provide the DOGMa estimations for the training/validation samples of the KITTI 3D object detection dataset. The test samples have no mapping to the raw sequences so their grids could not be obtained.

Out of the 7481 training/validation samples, we provide the corresponding DOGMas for 7275 of them. The missing 206 grids correspond to samples at the beginning of the raw sequences (where the filter had not yet converged), or to a sequence with odometry issues (0093@2011_09_26). The list of missing grids can be found in the following [file](grids_not_found_summary.log).

### Occupancy Grid size
The selected grid size was (432, 496) cells, with a resolution of 0.16 m. This corresponds to a distance of 69.12 m along the x axis (front of the vehicle) and 39.68 m to each side in the y axis. The coordinate frame of the grid is similar to that of the Velodyne sensor but located 1.73 m below in the z-axis (i.e. on the ground).

### Occupancy Grid contents
For each dataset sample, we provide two npy files:
- state_grid.npy: for each cell, it constains a discrete probability distribution over its occupancy state. It has four channels:
  - channel 0: static occupancy
  - channel 1: dynamic occupancy
  - channel 2: free occupancy
  - channel 3: unknown occupancy  
- velocity_grid.npy: for each cell, it constains the average velocity of its dynamic particles. The velocity is represented in cells per second. Note: in some corner cases (e.g. samples at the beginning of a sequence), there may be NaN or very large values in the velocities; your code should account for that. This file contains two channels:  
  - channel 0: velocity in the x axis
  - channel 1: velocity in the y axis  
  
Refer to the original CMCDOT publication (https://hal.archives-ouvertes.fr/hal-01205298/) for further details.  
The gif visualization above uses the following color scheme: blue (static), green (dynamic), red (unknown) and black (free). To help with the visualization, we approximately plotted the field of view (90 deg) of one of the front cameras.

We include in this repository some simple python code to visualize the grids. Additionally, the code used to generate the gif is also shared. 

## Utility
Occupancy Grids are an environment representation that is very convenient in tasks such as trajectory planning. In the paper "Leveraging Dynamic Occupancy Grids for 3D Object Detection in Point Clouds" (https://hal.inria.fr/hal-03044979/), we studied if and how this representation of the dynamic environment could help in the 3D object detection task.  

The conclusion of the paper was that by having the DOGMa information as an input to the detection model we could improve the orientation prediction for moving obstacles, as well as better detect smaller obstacles such as predestrians, especially when they are occluded or far away from the sensors (i.e. with fewer Lidar hits). In non-published experiments, this conclusion carried over to cases where the point cloud input was downsampled to a fewer number of layers.

## References
Please cite our work if you use our data:

```
@inproceedings{sierragonzalez:hal-03044979,
  TITLE = {{Leveraging Dynamic Occupancy Grids for 3D Object Detection in Point Clouds}},
  AUTHOR = {Sierra Gonz{\'a}lez, David and Paigwar, Anshul and Erkent, {\"O}zg{\"u}r and Dibangoye, Jilles S and Laugier, Christian},
  URL = {https://hal.inria.fr/hal-03044979},
  BOOKTITLE = {{16th IEEE International Conference on Control, Automation, Robotics and Vision (ICARCV)}},
  ADDRESS = {Shenzhen, China},
  YEAR = {2020},
  MONTH = Dec
}
```

```
@inproceedings{rummelhard:hal-01205298,
  TITLE = {{Conditional Monte Carlo Dense Occupancy Tracker}},
  AUTHOR = {Rummelhard, Lukas and Negre, Amaury and Laugier, Christian},
  URL = {https://hal.inria.fr/hal-01205298},
  BOOKTITLE = {{18th IEEE International Conference on Intelligent Transportation Systems}},
  ADDRESS = {Las Palmas, Spain},
  YEAR = {2015},
  MONTH = Sep
}
```

## Acknowledgements
Thanks to Toyota Motor Europe and Inria for supporting this work.
