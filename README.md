# SEP769 Deep Learning Project - Group 5 - LiDAR Sweeps
The code for this project is separated into different folders based on different stages of the project including Data Visualization, Data Preprocessing, Deep Learning Models Evaluations, and Trained Models. The final approach of the YOLOv3 model with 3D box regression which the team selected is referenced upon the open source lidar_dynamic_objects_detection method introduced by Dtananaev in his GitHub repository. The team has modified the original model and trained several models with different combinations of hyperparameters. The final detection results on LiDAR point cloud images of the best model are saved in the Final Detection Results folder.

## Description of the Method of the Final Approach

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


## Results of the Final Model
Loss Curve
<p align="center">
  <img src="https://github.com/markqian359/SEP769-DL-Project-LiDAR-Sweeps/blob/master/pictures/Loss%20Curve%20of%20the%20Final%20Model.png" width="1000"/>
</p>

Sample Detection Results
<p align="center">
  <img src="https://github.com/markqian359/SEP769-DL-Project-LiDAR-Sweeps/blob/master/Detection%20Results/79.png" width="900"/>
</p>
