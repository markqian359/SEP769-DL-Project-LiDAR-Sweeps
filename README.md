# SEP769 Deep Learning Project - Group 5 - LiDAR Sweeps
The code for this project is separated into different folders based on different stages of the project. The final approach which the team selected is based upon the lidar_dynamic_objects_detection method introduced by Dtananaev in his GitHub repository. The team has modified the original model and trained several models with different combinations of hyperparameters. The final detection results are saved in the Final Detection Results folder.

## The method

The lidar point cloud represented as top view image where each pixel of the image corresponds to 12.5x12.5 cm. For each grid cell
we project random point and get the height and intensity
<p align="center">
  <img src="https://github.com/Dtananaev/lidar_dynamic_objects_detection/blob/master/pictures/topview.png" width="900"/>
</p>
We are doing direct regression of the 3D boxes, thus for each pixel of the image we regress confidence between 0 and 1, 7 parameters for box (dx_centroid, dy_centroid, z_centroid, width, height, dx_front, dy_front) and classes.
<p align="center">
  <img src="https://github.com/Dtananaev/lidar_dynamic_objects_detection/blob/master/pictures/box_parametrization.png" width="1500"/>
</p>
We apply binary cross entrophy for confidence loss, l1 loss for all box parameters regression and softmax loss for classes prediction.
The confidence map computed from ground truth boxes. We assign the closest to the box centroid cell as confidence 1.0 (green on the image above)
and 0 otherwise. We apply confidence loss for all the pixels. Other losses  applied only for those pixels where we have confidence ground truth 1.0.


## The dataset preparation
We work with Pandaset dataset which can be uploaded from here: [Pandaset](https://pandaset.org/)
Upload and unpack all the data to dataset folder (e.g. ~/dataset).
The dataset should have the next folder structure:
``` bash
    dataset
    ├── 001                     # The sequence number
    │   ├── annotations         # Bounding boxes and semseg annotations
    |   |   ├──cuboids
    |   |   |  ├──00.pkl.gz
    |   |   |  └──  ...
    |   |   ├──semseg
    |   |      ├──00.pkl.gz
    |   |      └── ...
    │   ├── camera             # cameras images
    |   |  ├──back_camera
    |   |  |  ├──00.jpg
    |   |  |  └── ..
    |   |  ├──front_camera
    |   |  └── ...
    │   ├── lidar             # lidar data
    │   |    ├── 00.pkl.gz
    │   |    └── ... 
    |   ├── meta
    |   |   ├── gps.json
    |   |   ├── timestamps.json
    ├── 002
    └── ...
```
Preprocess dataset by applying next command:
```
cd lidar_dynamic_objects_detection/detection_3d/data_preprocessing/pandaset_tools
python preprocess_data.py --dataset_dir <path_to_your_dataset_dir>
```
Create dataset lists:
```
cd lidar_dynamic_objects_detection/detection_3d/
python create_dataset_lists.py --dataset_dir <path_to_your_dataset_dir>
```
This should create ```train.datatxt``` and ```val.datatxt``` into your dataset folder.
Finally change into ```parameters.py``` the directory of the dataset.
## Train
In order to train the network:
```
python train.py
```
In order to resume training:
```
python train.py --resume
```
The training can be monitored in tensorboard:
```
tensorboard --logdir=log
```
## Inference on validation dataset
In order to do inference on validation dataset:
```
python validation_inference.py --dataset_file <path_to_dataset_folder>/val.datatxt --output_dir <path_to_inference_output> --model_dir <path_to_trained_model>
```
The result of the inference is 3d boxes and also visualized 3d boxes on top view image. The visualized top view image (upper) concatenated with ground truth top view image (bottom).
