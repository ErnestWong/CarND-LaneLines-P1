# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[grayscale]: ./images/grayscale.jpg "Grayscale"
[gaussian]: ./images/gaussian.jpg "Gaussian"
[canny]: ./images/canny.jpg "Canny"
[hough]: ./images/hough.jpg "Hough"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale.
![alt text][grayscale], 
Then I used a gaussian blur filter. 
![alt text][gaussian]
Then, I used canny edge detection to get an image of the edges. 
![alt text][canny]
I used ROI to extract the regions of interest from the image, which is the bottom triangle portion of the image which contains the lanes. Then, I called hough lines to obtain all the lines in the image. 
![alt text][hough]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by filtering all the lines whose slope is less than 0.5 or greater than 2. These lines are disregarded because the slope indicates that the lines are likely outliers and not a part of the lane (the lane is sloped at roughly 30 degrees or 120 degrees). Furthermore, I iterate through the lines and organize them based on their slope to determine whether they belong to the left or right lane. Also, I include a heuristic which ensures that all left lines are on the left side of the image and all the right lines are on the right side of the image. Then, I use a polyfit function to obtain the line equation for each side. I extrapolate by selecting a start and end y coordinate on each side and find the exact coordinates using the corresponding line function on each side. I then return the two pairs of extrapolated coordinates which represent the left and right line to be drawn on the image.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen if the road conditions are different from the images/video. If there is rain or glare, the CV techniques used may not be effective, and we will probbably be unable to identify the lanes as well as in this exercise.

Another shortcoming could be that if there are other false lane lines due to anomalies in lane painting. If the lanes are painted in an unexpected fashion, this algorithm may not function as desired. Also, if there is debris or any marking on the road, the algorithm may incorrectly detect it as a lane line, which will cause issues.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to perform more pre-processing using CV algorithms. More pre-processing will allow the algorithm to be more robust in varying weather/lighting conditions.

Also, this algorithm is shown to work in sparse traffic. High traffic input may result in poor results, as a car in front of the image will be considered part of the Region of Interest. This may cause the lane detection algorithm to mess up. A possible improvement can be to update the region of interest so that it can handle heavy traffic.

Another potential improvement could be to improve the line-detecting algorithm (i.e. draw-lines). The current algorithm works well in the exercise inputs, but may not work for other situations. The algorithm can be adapted to be more robust such as having better filtering to remove non-lane lines.
