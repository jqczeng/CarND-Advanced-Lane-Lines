## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)

[distorted-undistorted-chessboard]: ./output_images/distorted-chessboard.png "Distorted vs. Undistorted"
[lane-fit]: ./output_images/color-fit-lines.jpg "Lane Line Fit"
[thresholded]: ./output_images/original-threshold.png "Original vs. Thresholded"
[birds-eye-1]: ./output_images/birds-eye-view-1.png "Bird's Eye View Lane Fitting 1"
[birds-eye-2]: ./output_images/birds-eye-view-2.png "Bird's Eye View Lane Fitting 2"

In this project, the goal is to write a software pipeline to identify the road lane boundaries in a video. The radius of curvature for the lanes and as well as the car position are to be shown in the video as well. 


Camera Calibration:
---
Image distortions can affect the processing algorithms aimed at extracting information. Distortions can:
  * Change the apparent size of an object in an image
  * Change the apparent shape of an object in an image
  * Cause an object's appearance to change dpending on where it is in the field of view
  * Make objects appear closer or farther away than they actually are

Thus, a distortion correction needs to be applied to the raw images. The computer vision library OpenCV contains functions to calibrate the camera and for distortion correction. The functions are called `cv2.calibrateCamera()` and `cv2.undistort()`, respectively. A series of chessboard images are used. Chessboard images are used because of its distintive and easy-to-detect corner points as well as its high constract color between the squares. Also, the undistorted 2D image of a chessboard is known.

![alt text][distorted-undistorted-chessboard]

The distorted and undistorted images can be seen above. By applying the distortion correction algoirthm, the edges of the chessboard squares are straight like it should be.

Threadholded Binary Image:
---
A threshold filter is applied to each image after distortion correct. The thresholding is used to create a constrast within the image between objects and as well to filter out edges so that it will be easier to detect the lanes. There are two thresholds combined together; a gradient threshold and a color threshold.

A gradient threshold uses the Sobel operator to find the pixels within an image where there are is a change in intensity from one pixel to another. The gradient can be taken either in the x-drection or the y-direction of the image. The gradient is a vector and thus its direction and magnitude parameteres will indicate the intensity of change at each pixel, and infer how likely it is that that pixel represents an edge.

After the thresholds are applied to the image, the image is filtered and is ready to be used for lane detection. 

![alt text][thresholded]

Perspective Transform:
---
The perspective transform transforms a 3D image into a "2-D" birds-eye view of the road. The lanes can then be detected easier in a 2D image instead of a 3D image. The perspective transform is also able to determine the curvature of the lane and vehicle position with respect to center. 

![alt text][birds-eye-1]
![alt text][birds-eye-2]

Lane Detection:
---
A histogram indicates where the lane lines start and can be used as a starting point for determining where the lane lines are. Next, a sliding window algorithm finds the location of the line at each section within the image. All of the pixels that belong to the left and right lane line are aggregated together and a line polynomial is fitted. 

![alt text][lane-fit]

Future Work:
---
There are several improvements to the pipeline that can be implemented to further enhance lane detection. First, a moving average of lane line fits can help filter out lane fit anomalies. Secondly, a sanity check can be implemented in order to check if the fitted lane line is correct. This can be done by comparing the currently fitted lane line to the history of fitted lane lines. 


