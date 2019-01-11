## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  

A great writeup should include the rubric points as well as your description of how you addressed each point.  You should include a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project:
---
adsf

Camera Calibration:
---
Image distortions can affect the processing algorithms aimed at extracting information. Distortions can:
  * Change the apparent size of an object in an image
  * Change the apparent shape of an object in an image
  * Cause an object's appearance to change dpending on where it is in the field of view
  * Make objects appear closer or farther away than they actually are

Thus, a distortion correction needs to be applied to the raw images. The computer vision library OpenCV contains functions to calibrate the camera and for distortion correction. The functions are called `cv2.calibrateCamera()` and `cv2.undistort()`, respectively. A series of chessboard images are used. Chessboard images are used because of its distintive and easy-to-detect corner points as well as its high constract color between the squares. Also, the undistorted 2D image of a chessboard is known.

Threadholded Binary Image:
---
A threshold filter is applied to each image after distortion correct. The thresholding is used to create a constrast within the image between objects and as well to filter out edges so that it will be easier to detect the lanes. 

Perspective Transform:
---
The perspective transform

* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

