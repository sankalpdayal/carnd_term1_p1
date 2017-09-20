# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./test_images_output/gray.png "Grayscale"
[image2]: ./test_images_output/blur_gray.png "blur gray"
[image3]: ./test_images_output/masked_edges.png "masked edges"
[image4]: ./test_images_output/line_image.png "line image"
[image5]: ./test_images_output/image_lanes.png "image lanes"


[//]: # (Link References)

[git repo]: https://github.com/udacity/CarND-LaneLines-P1
[wiki link]: https://en.wikipedia.org/wiki/Least_squares#Weighted_least_squares
[prob hough transform]: https://medium.com/@esmat.anis/robust-extrapolation-of-lines-in-video-using-linear-hough-transform-edd39d642ddf
[color transformation]: https://medium.com/towards-data-science/finding-lane-lines-on-the-road-30cf016a1165

This project is first project in term1 of the Self Driving Car Nano Degree. The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report

The starter code for this project was taken from here [git repo] 

This project contains following folders and files

P1.ipynb                 : Notebook demonstrating the lane detection pipeline.

examples                 : On how the algorithm's final output should look like

test_videos, test_images : Test videos and images

test_videos_output       : Output for the test videos

### Reflection

### 1. Description of the pipeline

My pipeline consisted of 6 steps. 

1. First, I converted the images to grayscale. This will allow detection of yellow and white lanes both using the same image.

2. Then I apply gaussian smoothening so that no sharp and prominant objects get smoothened out.

3. Then I apply canny edge detection algorithm. This allows edges of lanes to be detected easily.

4. This canny edge image is then masked with a polygon to allow only those edges that correspond to possibly edges of lanes.

5. Then this masked image is then given to hough transform to get only the lines that satisfy minimum length and maximum line gap requirements.

6. Out of these lines, left and right lanes are identified. This is done in the method 'get_left_right_lanes'.
	
	1. Depending on the slope of the lines, two lists of lines are created corresponding to left and right lanes. In this list x1,y1,x2,y2 are stored.
	
	2. For each lane, a line is fitted passing through the points in the list. This fitting is done using weighted least squares method [wiki link]  
	
	3. The weights are defined using the length of line. This is done in order to trust lines that are longer in length.
	
Following images demonstrate how the pipeline works

1. Conversion to grayscale  
![alt text][image1]

2. Gaussian smoothening 
![alt text][image2]

3. Masked canny edge detection 
![alt text][image3]

4. Lines from hough transform 
![alt text][image4]

5. Left and right lanes after weighted least squares fitting 
![alt text][image5]


### 2. Potential shortcomings with the current pipeline

One potential shortcoming is that the since it is solely based on detection of edges, it can be enchanced by considering color of lanes.

Second issue with the pipeline is that it assumes that both left and right lane will have slopes with different signs. This might not be true in cases where car needs to take a turn.

Another potential issue is the lanes are assumed within a polygon of image, this again might not be ture when car needs to take a turn.

### 3. Some possible improvements to the pipeline

A possible improvement would be to apply more probabilisitc prediction of lanes using the method as described by Esmat Nabil [prob hough transform].

Another improvement could be using better color based lane extraction as done by Naoki Shibuya [color transformation]