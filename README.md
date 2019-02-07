# Purpose

**To develop and train a convolutional neural network (CNN) model for autonomous driving.**


# Executive Summary

Udacity built a car simulator for Udacity's Self-Driving Car Nanodegree course and was released to the public in 2016. The simulator contains one simple track and one complex track and it allows the user to collect the center, left, right images with other information like steering angle, speed, break and etc. From the data collected from the simulator, the purpose of the project is to build a convolutional neural network (CNN) model that takes in the center, left and right images and predict the steering angle.


# Method

1. Obtain and check the data
2. Apply image augmentation
3. Build convolutional neural network model
4. Check the loss and validation loss
5. Test the model


# Data

- Check out the [Udacity's car simulator](https://github.com/udacity/self-driving-car-sim)
- Record each tracks (Produces two files for each track)

**driving_log.csv (Driving information)**

|Feature|Type|Description|
|---|---|---|
|Center|Object|Path to the center image file| 
|Left|Object|Path to the left image file| 
|Right|Object|Path to the right image file|
|Steering|Float|Steering angle (left: negative, right: positive)| 
|Throttle|Float|Acceleration of the car| 
|Break|Int|Breaking of the car| 
|Speed|Float|Speed of the car| 

**IMG (160 X 320 images recorded)**

- Track 1
Center                         |Left                       |Right
:-----------------------------:|:-------------------------:|:------------------------------
![](/image/center1.png)        |![](/image/left1.png)      |![](/image/right1.png)


- Track 2
![Center](/image/center2.png) ![Left](/image/left2.png) ![Right](/image/right2.png)


# Steering Angle Correction

In order to account the left and right camera position being off centered, the steering angle should be correct. Testing from no correction to +/- 0.25, the best correct was +/- 0.15 for both tracks.

![Left (Before)](/image/left1ang.png) ![Left (After)](/image/left1cor.png)

![Right (Before)](/image/right1ang.png) ![Right (After)](/image/tight1cor.png)

# Image Augmentation

The more data the model train on, the better the performance. Since I only recorded 1 lap of the track, the model might not have enough information to run and make mistakes on the steering angle prediction. One of the ways to fix the limited data problem with images is to perform the augmentation process. Image augmentation artificially creates training images through different ways of processing or combination of multiple processing, such as random rotation, shifts, shear, and flips, etc. In this specific example, the images are flipped and the corresponding steering angles are inversed. This process will double the data I originally obtained from the simulation. 

***Flipped Image Example**
- Track 1
![Left (Before)](/image/left1cor.png) ![Left (After)](/image/left1aug.png)

- Track 2
![Right (Before)](/image/right1cor.png) ![Right (After)](/image/right1aug.png)


# Convolutional Neural Network (CNN)

**Model Structure**

|Layer|Output Shape|Param #|
|---|---|---|
|Cropping 2d|(None, 65, 320, 3)|0| 
|Lambda|(None, 65, 320, 3)|0| 
|Conv 2d|(None, 31, 158, 24)|1824|
|Conv 2d|(None, 14, 77, 36)|21636|
|Conv 2d|(None, 5, 37, 48)|43248|
|Conv 2d|(None, 2, 18, 64)|27712|
|Flatten|(None, 2304)|0|
|Dropout|(None, 2304)|0|
|Dense|(None, 100)|230500|
|Dropout|(None, 2304)|0|
|Dense|(None, 50)|5050|
|Dropout|(None, 50)|0|
|Dense|(None, 1)|51|

Total params: 330,021
Trainable params: 330,021
Non-trainable params: 0

# Model Evaluation

**Track 1**
- Mean Squared Error
    - Loss: 0.0192
    - Validation Loss: 0.0303
    
**Track 2**
- Mean Squared Error
    - Loss: 0.0877
    - Validation Loss: 0.0872


# Model Performance

[**Track 1**](https://youtu.be/GJ8sPhd0GcE)

[**Track 2**](https://youtu.be/s_gfxrp02XQ)




