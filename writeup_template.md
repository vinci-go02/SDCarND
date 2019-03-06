# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRight_final.jpg "White Lane"
[image2]: ./test_images_output/solidYellowCurve_final.jpg "Yellow Lane"
[image3]: ./test_images_output/whiteCarLaneSwitch_final.jpg "Lane Switch"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In this project, I used Python and OpenCV libraries to identify and track lane lines on the road/highway given in test images and test videos. 

Following pipeline was developed as I worked through my code and tested a series of individual images, and later applied it to test videos.

1. Read test images.
2. Apply Grayscale conversion.
3. Apply Gaussian Smoothing.
4. Apply Canny edge detection.
5. Define a region of interest OR mask.
6. Apply Hough lines transform.
7. Average and extrapolating the lane lines.
8. Test images.
8. Apply the pipeline on test videos.

Canny Edge Detection:
    This is an 'edge detection' helper that involves the following steps:
        1. The algorithm measures the intensity gradients of each pixel. So, we convert the images into gray scale in order to detect edges.
        2. Since the results get affected by image noise, it is essential to filter out the noise to prevent false detection caused by noise. So, a Gaussian smoothing is applied.
        3. Define low_threshold and high_threshold for the difference in intensities of adjacent pixel values.
        4. If an edge pixel’s gradient value is higher than the high threshold value, it is marked as a strong edge pixel. 
        5. If an edge pixel’s gradient value is smaller than the high threshold value but larger than the low threshold value, it is marked as a weak edge pixel. 
        6. If an edge pixel's value is smaller than the low threshold value, it is ostracized. 
        7. The two threshold values are empirically determined and their definition will depend on the content of a given input image.

Hough Transform:
    This technique is used to isolate features of a particular shape (contour etc.) within an image.

Averaging and Exrtrapolating Lines:
    1. As we have multiple small lines detected for each lane line, we need to average all these lines (using their slopes and end-points obtained from Hough Transform) and draw a single line for each lane line. 
    2. We also need to extrapolate the lane lines to cover the full lane line length.
    
    To Do this: (Think Mathematically, In reality, this will be Reverse in Images and Videos)
        a. Left lane lines will have a Positive slope while the right lane will have a negative slope, from Mathematical Cartesian coordinate system.
        b. So, Segregate positive slope lines and negative slope lines. Add slopes, x-coordinates, y-coordinates for each of Left line and Right line classes, and count such number of lines in each class. Take average of all the quantities.
        c. Calculate bottom and top x-coordinates for Left and Right lanes from the calculated averages assuming that corresponding y-coorindates are fixed or taken according to test-images and test-videos.
        d. Draw full length lane lines now.

You may check my results for some images here:

![alt text][image1]
![alt text][image2]
![alt text][image3]

Next, I used the above pipelines to detect lanes lines in the test videos. The outputs for both the images and videos are in respective folders in the project repository.

### 2. Identify potential shortcomings with your current pipeline

The project succeeded in detecting the lane lines with mostly accurate line fits in the video streams, except for the 'Challenge' video.
There are possible few reasons I can think of to answer why my lane detection pipeline failed in the 'Challenge':
  
  1. The detection is mainly accustomed to lighting effects. So, Night time or weahter like Rainy/Cloudy/Snowy day can easily interfere   with.
  2. We have mostly hard-coded the region of interest in the project as the results shall easily fail to match our prediction with any slight changes in the camera position etc.
  3. The surroundings also play a good role like Tress shadows falling on the highway, there can be a lot of traffic while in the given tests, there are little or no traffic except for the 'Challenge' video as some features of other cars in the traffic may also be detected in the masked region.
  4. How the road form changes in between also can mess with results with this code.

### 3. Suggest possible improvements to your pipeline

 1. As this project is clealy intended to only detect straight lines, a possible improvement would be to modify the above pipeline to detect curved or bendy lane line. 
 2. Another potential improvement could be to identify and define lane types using machine learning techniques.

The project was initial boost to go ahead with the Nanodegree.