
# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: dotted_lines.jpg 
[image2]: solid_line.jpg 



---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

(1) Pipeline: 

The pipeline consists of following stages:

Stage1: Converted colored image to graysale.

Stage2: Applied gaussian smoothing on the image from stage1.

Stage3: Applied canny for canny edge detection on the images obatained from stage2.

Stage4: Marked the region of interest. Used polygon/quadrilateral shape for the region and defined the vertices for it.
Masked the area outside of the region of interest from the imsge obtained frrom stage3.

Stage5: Applied hough transform logic to draw the lines for left side of the lane and right side of the lane for the path using image from stage4. Used draw_lines() function to draw the lines identified by hough lines algorithm.

However, it did not give the single solid line for the side of the path.
Following is the exmaple:

![alt text][image1]



Stage 6: Draw the lines identified in stage 5 on the original input image.


(2) draw_solid_eoe_line function:

To find the single solid line for each side of the lane, implemented another draw function called draw_solid_eoe_line function. 

The algorithm is to extrapolate the end points for the left and right sides of the lane using segmented lines detected by hough lines:

0] Initialize left(x, y) and right(x, y) vertices.

1] For every x1, y1, x2 , y2 in every line:
 Calculate all the slopes and intercept using equation y = mx+b
  slope = m = (y2 - y1)/(x2 - x1)
  intercept = b = (y1 - (m * x1))
  
 Change the default value of left_top(x, y) and right_top(x, y) based on the values for x1, y1, x2 ,y2.
 For exmaple, if new x1 is smaller than left(x) change left(x).
 
2] Calculate the avg_slope and avg_intercept for both left side and right side.

3] Calculate left side's bottom vertex's x value using equation y = mx+b where y is the max y of the image.

4] Calculate right side's bottom vertex's x value using equation y = mx+b where y is the max y of the image.

![alt text][image2]




### 2. Identify potential shortcomings with your current pipeline


(1) The curves/sharp curves handling: If there is a curve, then using quadrilateral as region of interest used for images/videos where path is mostly straight does not work.



### 3. Suggest possible improvements to your pipeline

(1) The curves/sharp curves handling: In case of curves, the region of interest chosen using quadrilaternal can cause selection of the wrong region or short path region. Instead of using line equation, equation for the curve can be used. Instrad of drwaning lines, curves need to be drawn.
