# 3D Object Detection

## Display of 10 different vehicles

In the next captures you find multiple vehicles with in the first one we can identify 10 vehicles half of them are veiwed from the front side and the other half is seen from the rear side.

![alt text](/img/pcl_images_front.PNG "PCL capture front")

In the capture bellow we can observe 3 vehicles from the side

![alt text](/img/pcl_images_rear.PNG "PCL capture side")

## Observable features

All the obersable are listed bellow and enlighted by red circle on the PCL image.

Features:
- wind shield
- tires
- front bump
- rear bump
- mirrors
- rooftop

![alt text](/img/pcl_images_rear.PNG-mh.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_0.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_1.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_2.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_3.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_4.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_5.png "PCL capture features")
![alt text](/img/pcl_images.PNG-mh_6.png "PCL capture features")


# Track 3D-Objects Over Time

Please use this starter template to answer the following questions:

### 1. Write a short recap of the four tracking steps and what you implemented there (filter, track management, association, camera fusion). Which results did you achieve? Which part of the project was most difficult for you to complete, and why?


### 2. Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)? 


### 3. Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?


### 4. Can you think of ways to improve your tracking results in the future?

## Filter

In this step a Kalman filter was implemented with both prediction and update step. At this step only LIDaR measures are included.
The result for this step in one confirmed tracked close to the groundtruth. The mean RMSE is 0.32.

![all text](/img/EKF.PNG)

![all text](/img/EKF_RMSE.PNG)

## Track Management
The second step was to manage one track, from creation for a track, updating its score, changing it's state and deleting it.
In this step a track went from initialized to tentative and finaly confirmed. The RMSE is slowly decreasing and its mean is 0.78.

![all text](/img/Step2_RMSE.PNG)

## Association
This step is about managing mutliple tracks and associate the measure with the closest track. To assiociate a measure to a track the Malhanobis distance is computed for each track and measure combiantion. The Malhanobis distance is a metric based and the distance between the measure and the track and covariance matrix of the track. A previous step to association is the gating, to avoid unlikely measure to be associate with a track by a remaining unassigned track a threshold is defined. The threshold is defined with inversed cumulative Student density law.

![all text](/img/Step3.PNG)
![all text](/img/Step3_RMSE.PNG)

RMSE per track:
- 0: 0.15
- 1: 0.12
- 10: 0.19

In the next step we'll add the camera measurements and observe the effect it has on the RMSE as it is our metric to compare tracking performances.

## Camera Fusion
At this step the camera measurement were added at this step. This was done by implementing camera measure covariance matrix and model matrix. You'll find the result video inside the result folder.

![all text](/img/Final_RMSE.PNG)

RMSE per track:
- 0: 0.17
- 1: 0.10
- 10: 0.14

For track 1 and 10 the RMSE has improved but not significantly. Which means that both of camera and LIDaR are providing measurement of approximatly the same quality.

On the side of the track 0 the RMSE has increased which should not be the case. This could be caused by a degradation of the measure provided by a camera. Those degradation can come from a poor measure from the camera, an measure offset, a bad estimation of the error on the camera sensor, ...

## Fusion Benefit
If well parametrized a fusion algorithm fuses data from different sources to give a better approximation of the reality than by taking each sensor separatly. In this case not all of the tracks were improved but the downgrade in terms of performances is acceptable. Also using multiple sensors fusion gives a combination of the qualities of various sensors to get the better approximation of reality.

## Fusion Challenge
In this course we've covered the basics of KF, Kalman Filters are about fine tuning and adjust the covariances parameter, study the measurement matrix. In this case we considered a case where the sensors informations are known, we approximate the movement of the targets by a straight line in space, sensor are unbiased and we know our systems modelisation. All these points are challenges that a KF could encounter.

## Improvement
We could add more sensors to improve measurement or simply adding some measure. 

Improvement ideas:

- add radar data to fusion to add a measure on close object depth.
- add sensors for data redundancy and improved approximation in the way it done with GPS and IMU combination.

## Difficulty
I personaly already worked with KF for GPS position enhancement with IMU and embedded sensor. The Kalman filter part was cristal  clear to me. The most difficult part must then be the association of multiple tracks.
