## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)

[distorted-undistorted-chessboard]: ./output_images/distorted-chessboard.png "Distorted vs. Undistorted"
[lane-fit]: ./output_images/color-fit-lines.jpg "Lane Line Fit"
[thresholded]: ./output_images/original-threshold.png "Original vs. Thresholded"
[birds-eye-1]: ./output_images/birds-eye-view-1.png "Bird's Eye View Lane Fitting 1"
[birds-eye-2]: ./output_images/birds-eye-view-2.png "Bird's Eye View Lane Fitting 2"
[detected-lane-1]: ./output_images/lane-detected-1.png "Detected Lane Area 1"
[detected-lane-2]: ./output_images/lane-detected-2.png "Detected Lane Area 2"
[windowing-1]: ./output_images/windowing-1.png "Sliding Window Detection 1"
[windowing-2]: ./output_images/windowing-2.png "Sliding Window Detection 2"

In this project, the goal is to write a software pipeline to identify the road lane boundaries in a video. Each video image is filtered and converted into a high-contrast, binary image. The image is then transformed from a 3D image to a 2D image to curve fit the lane lines. The radius of curvature for the lanes and as well as the car position are then calculated. The fitted lane lines are then transformed back to the 3D image with the radius of curvature and car position displayed on the image. The result is a pipeline that can detect lane lines, straight or curved. 

The following sections describe in detail the steps to detect lane lines. 

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
The perspective transform transforms a 3D image into a "2-D" birds-eye view of the road. The lanes can then be detected easier in a 2D image instead of a 3D image. The perspective transform also helps determine the curvature of the lane and vehicle position with respect to center. The bird's eye view can be seen in the pictures below. 

![alt text][birds-eye-1]
![alt text][birds-eye-2]

Lane Detection:
---
A histogram indicates where the lane lines start and can be used as a starting point for determining where the lane lines are. Next, a sliding window algorithm finds the location of the line at each section within the image. The first window starts at where the histogram peaks are. Each peak represents one lane line. The window is a defined area which encapsulates a portion of the lane. The contrasted pixels within in the window which represents the lane lines are found. The window then moves to the next area of the image by moving the vertical distance of a single window. Lane lines are continuous lines, so in the next window, a margin in the X-direction can be used to find the lane line pixels. The image is split into several windows due to the curvature and vertical height. The windowing algorithm can be seen in the following images.

![alt text][windowing-1]![alt text][windowing-2]

After the windowing algorithm step has been carried out, a second order polynomial line is fitted for both lane lines. This can be seen in the picture below. 

![alt text][lane-fit]

Putting it all togther:
---
By using all of the forementioned tools, the lane lines of a road can be detected. The detected area and lane lines can be seen in the following pictures. The green area represents the lane area in front of the vehicle. The red lines overlaying the white lane lines represents the detected lane lines.

![alt text][detected-lane-1]![alt text][detected-lane-2]

The resulting video can be found in "project_video_with_lanes.mp4". The challenge video was also tested with the pipeline. However, there are several shortcomings which will be described in the future works section.

HTML("""
<video width="960" height="540" controls>
  <source src="{0}">
</video>
""".format())

Future Work:
---
There are several improvements to the pipeline that can be implemented to further enhance lane detection. First, a moving average of lane line fits can help filter out lane fit anomalies. Secondly, a sanity check can be implemented in order to check if the fitted lane line is correct. This can be done by comparing the currently fitted lane line to the history of fitted lane lines. 


