# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/OutputImage1.jpg "canny image"
[image2]: ./test_images/OutputImage2.jpg "Houghline image"
[image3]: ./test_images/OutputImage3.jpg "Final image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I i filtered the image using the

**gaussian_blur()** function. I then ran **canny()** to get the Canny edges image. See below:
![alt text][image1]

To only filter in the road ahead I ran **region_of_interest()**. I then ran **cv2.HoughLinesP()** to get all lines in the region of interest. 
See below: ![alt text][image2]

In order to draw a single line on the left and right lanes, I created a function **HoughLinesToLeftAndRightLane(lines, ImgShape)** that took the all the Hough lines and the image shape, and returns an image with left and right lane lines drawn onto it.
It divides the lines into left and right lane lines based on the line slope compared to the average line slope. These line slopes and biases are averaged to get the best estimate.
A filter is also implemented that filters out lines with slopes that deviate too much from an expected minimum slope. 

Then the **weighted_img()** is used to merge the original image with the Lane-line image.
See below: ![alt text][image3]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what happens when only one lane line is seen, or one is hardly visible. This would
cause the algorithm to calculate the left and right lane lines ontop of eachother. This can be seen in the challenge.mp4, in the midle of the clip.   
This happens as the algorithm, given more than 5 Hough lines, is forced to calculate a left and a right lane-line. 

Another shortcoming could be lighting conditions have to be good enough for the algorithm to detect lines at all. Right now it will not
output any lines if the number of Hough lines is below 5.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to check all hough lines starting point to improve the categorization algorithm in left and right lane lines.

Another improvement is to adaptively change thresholds on **cv2.HoughLinesP()** function until a certain number of HoughLines is found. This would improve tolerance for lighting conditions.  

A colour filter could also be used, that was sensitive to white and yellow colours to better filter in lanes.

