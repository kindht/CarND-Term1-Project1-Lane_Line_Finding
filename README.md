# **Finding Lane Lines on the Road** 



**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)



---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.  

Step1   Converted the images to grayscale  
Step2  Apply Gaussian smoothing  
Step3  Apply canny detection to get edges  
Step4  Create a masked image by defining region of interest  
Step5  Apply hough transforms to draw line segments with original draw_lines(), then draw 2 lanes with modified draw_lines() 
Step6  Aplly weighted images to combine initial image with the lanes image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by :

1.  Calculate slope of line segments by fitting lines (y=Ax+B)  where A is the slope
2.  Separate left and right lines: if slope is negative , then it is a left line, otherwise it is a right line;  using -0.1 and 0.1 as slope threshhold  in order to get rid of lines that are almost horizontal , since there were some there that impact the results
3.  Get rid of lines that are not on actual left or right lanes by looking at their X coordinates , since there were some line segments with positive slopes, but on the left in the image
4.  Calcuate the average slope(A) and offset(B)  
5.  Calcuate x coordinates of end points for left and right lines respectively, given y coordinates (apex is defined according to ROI )

It took me lots of time tuning the parameters at the begining , and finally found those in course exercises just work quite well.

#### 2 paremeters are very impressive:
1. min_lin_len for Hough transforms, orignially I set it as 10, that resulted in some very short lines which impacted final results, after change to 40, results are much better

2. the upper limit of the mask, orginally I set a apex with  y value of 320,  that resulted in many annoying lines, after change it to 325 ,  the results become much cleaner

![alt text][image0]  

[image0]: ./pipe-line-images/apex-example.png 



Following to show how the pipeline works:

![alt text][image1]  

[image1]: ./pipe-line-images/solidWhiteRight_gray.jpg  

![alt text][image2] 

[image2]: ./pipe-line-images/solidWhiteRight_canny.jpg  

![alt text][image3] 

[image3]: ./pipe-line-images/solidWhiteRight_masked.jpg  

![alt text][image4] 

[image4]: ./pipe-line-images/solidWhiteRight_segline_img.jpg 

![alt text][image5] 

[image5]: ./pipe-line-images/solidWhiteRight_line.jpg 



### 2. Identify potential shortcomings with your current pipeline


One shortcoming is that the lanes show on videos seem still a bit trembling

Another shortcoming could be that lanes can not be drawn prefectly when there are more curves


### 3. Suggest possible improvements to your pipeline

A possible improvement would be implement a better algorithm for curved lanes ,  i.e. to figure out a better way to calculate  average slope for each lane and better fit them

Another improvement could be using cv2.inRange() to assist the lane detection, while I am not quite sure about it yet

