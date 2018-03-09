## **Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/chessUdistort.PNG "Undistorted"
[image2]: ./examples/distort_correct.PNG "Road Transformed"
[image3]: ./examples/combined_threshold.PNG "Binary Example"
[image4]: ./examples/warpped.PNG "Warp Example"
[image5]: ./examples/lanes.PNG "Fit Visual"
[image6]: ./examples/finalImage.PNG "Output"
[video1]: ./project_video_output.mp4 "Video"

### [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points



### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the third code cell of the IPython notebook 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. 
The value of `x is 9` and `y is 6`. will appended `objpoints` and `imgpoints` every time I successfully detect all chessboard corners in a test image.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

Total 3 Images  which were not included for calibration as x and y didn't matched to 9 and 6 respectively.

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

Details can also be viewed under .html file for better visualization.
#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps in jupyter notebook at code block 6 to code block 11 ,   Here's an example of my output for this step.  

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is in jupyter notebook under cell 12 and visulaization under cell block 13 .  I chose the hardcode the source and destination points in the following manner:

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 587, 454      | 243, 0        | 
| 694, 454      | 1055, 0       |
| 1055, 685     | 1055, 685     |
| 243, 685      | 243, 685      |

I verified that my perspective transform was working as expected by drawing the `src`  points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I then perform a sliding window search, starting with the base likely positions of the 2 lanes, calculated from the histogram. I have used 9 windows of width 200 pixels.The logic is similar to the one provided in classroom.

The x & y coordinates of non zeros pixels are found, a polynomial is fit for these coordinates and the lane lines are drawn. Below is the visualization. Code can be found under jupyter notebook in cell 14,15

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.


The radius of curvature is computed according to the formula and method described in Udacity classroom. Since we perform the polynomial fit in pixels and whereas the curvature has to be calculated in real world meters, we have to use a pixel to meter transformation and recompute the fit again. Conversion details are taken from classroom.

The mean of the lane pixels closest to the car gives us the center of the lane. The center of the image gives us the position of the car. The difference between them is the offset from the center.

The code an be seen under cell 15 of jupyter notebook

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in pipeline function in code block cell 16 .  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Issues :
The major issue i faced was color and gradient thresholdings , had to apply hit and trial across various threshold values , joining combination and color spaces to detect maximum portion of lane lines.
I also faced issue while selecting source and destination points for perspective transformation , but managged to get fair good points by looking at sample images on scales , was not able to find to get those dynamically.

Drawbacks and Ways to overcome :
My pipeline would fail in case lane lines are not detected accurately or have sharpe turns or drastic change in birghtness . To over come this i could have use averaging techniue to average out last few frames and add check point to use old frame in case lane lines were not detected.
