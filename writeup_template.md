#**Finding Lane Lines on the Road** 

[//]: # (Image References)

[canny_edges]: ./results/canny_edges.png "Canny Edges"
[hough_lines]: ./results/hough_lines.png "Hough Lines"
[lane_lines]: ./results/lane_lines.png "Lane Lines"
[region_of_interest]: ./results/region_of_interest.png "Region of Interest"
[result]: ./results/whiteCarLaneSwitch.png "result"

### Reflection

###1. Description of Pipeline

My pipeline consisted of 6 steps. I first converted the raw image to grayscale, then applied a slight Gaussian blur. With the resulting image, I applied the Canny transform with a low threshold of 50 and high threshold of 200. The resulting edges are shown below.

![canny_edges.png][canny_edges]

From there, I applied a region of interest mask to the lower center triangular region of the image. This region of interest is shown below on the original image.

![region_of_interest.png][region_of_interest]

I then applied the Hough Line Transform to find straight lines within the region of interest.

![hough_lines.png][hough_lines]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to take an average of the lines returned from the Hough transform. By categorizing the lines by their slope, I associated the positively sloped lines with the left lane line and negatively sloped lines with the right lane line. To filter out effectively horizontal lines that may appear in image, I only took lines with slopes greater than 0.5 or less than -0.5. 




###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...