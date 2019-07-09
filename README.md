## Advanced Lane Finding Project ##

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

[image1]: ./camera_cal/calibration1.jpg "Distorted"
[image2]: ./output_images/Camera_cal/chessboard0.jpg "Undistorted"
[image3]: ./test_images/test1.jpg "Distorted test image"
[image4]: ./output_images/test_undist0.jpg "Undistorted test image"
[image5]: ./output_images/sobel0.jpg "Sobel filter output"
[image6]: ./output_images/warped/warped4.jpg "warped"
[image7]: ./output_images/lane_line/lane_line.jpg "Output"
[image8]: ./output_images/lane_line/lane_line0.jpg "Output_curve"
[video1]: ./project_video.mp4 "Video"




### Camera Calibration

#### 1. Calculation of camera matrix and distortion coefficients

The code for this part is in [Calibration.ipnb](./Calibration.ipynb).
I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. 
the inside corners are counted be 9 and 6,
then using cv function calibrateCamera() , distortion coefficients were calculted .

the distortion coeeficient is also saved for future use .

Below is the image of chess board before and after undistorting
**Original**
![alt text][image1]
**Undistorted**
![alt text][image2]

Rest of the chess board calibration can be found in the [Output Folder](./output_images/Camera_cal/)



### Pipeline (single images)

#### 1. Distortion-correction of test image.

The distortion coeffiecent is applied on the test images below is the result

**Distorted test image**
![alt text][image3]

**Undistorted test image**
![alt text][image4]

#### 2. color transforms, gradients or other methods to create a thresholded binary image


The undistorted image in now passed through sobel filter and a x and y derivative is obtained.followed by a colour transform is performed into HLS and HSV colur space .

among which s channel and v channel is choosen for future analysis as this had better visibility of yellow lanes when transformed .

the code for below part is in [Image.ipnb](./Image.ipynb)

The final transformed image is below
![alt text][image5]




#### 3. Perspective transform and provide

The code for my perspective transform includes a function called `warp()`, which appears in the file `image.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warp()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
    src = np.float32([
        [180,700],
        [600,450],
        [1100,700],
        [720,450]
    ])
    
    dst = np.float32([
        [270,700],
        [270,0],
        [1150,700],
        [1150,0]
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
|  180, 700     | 270, 700      | 
| 600, 450      | 270, 0        |
| 1100, 700     | 1150, 700     |
| 720, 450      | 1150, 0       |

 my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

 The warped image output is below

![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Using the slinding window method the image is divided to 9 windows and histogram is obtained for each of the window , the lane line in each window is identified and moved to the next window until the end of the image and a line is drawn at the end connect these lines . followed by that an inverse transform is performed on the image also the region between the image is drawn with green colour

![alt text][image7]

#### 5.Calculation of the radius of curvature of the lane and the position of the vehicle with respect to center.

The curvature is found by left and right lane indices are fitted to a 2 degree polyomial. by using radius of curvature formaula the left and right radius is calculated and it is added to the image

![alt text][image8]

### Pipeline (video)


the Video output is here 
[link to my video result](./test_videos_output/project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

1. The Radius of curvature can be more accurate predecting more accurate numbers

2. The Model fails to identify the lane when the colour of the road has different contrast and any tyre marking, dirt in it or rain fall are present in the road

3.The Model also fails when curve is steep like the forest area road with short curves and lot of shade region 

4.Deep Neural Networks can be used to make this model better as this provides a solution to train the module in various situation like snow ,rain , dirt etc the network would learn all of these possible scenario and peroform better ( however getting the right kind of labeled data is also a challenge)



