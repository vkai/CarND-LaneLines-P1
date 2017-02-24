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

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to take an average of the lines returned from the Hough transform. By categorizing the lines by their slope, I associated the positively sloped lines with the left lane line and negatively sloped lines with the right lane line. I then averaged the coordinates of each lane to calculate an average pair of points to represent the average line. To filter out lines that were not likely part of the lanes, I only included lines with slopes greater than 0.5 or less than -0.5. With the averaged pair of points, I was able to compute the slope and intercept, and apply a standard line formula to points based on the dimension of the orignal image.

![lane_lines.png][lane_lines]

I then used the weighted_image() function to overlay the averaged lane lines over the original image.

![result.png][result] 


###2. Potential Shortcomings

One potential shortcoming with my approach would be if the vehicle approached a 90-degree turn with lane lines. Based on my naive line filtering logic, the lane lines on the perpendicular road would not be recognized as a lane because their slope would be within -0.5 and 0.5, even close to a slope of 0. 

A couple shortcomings are apparent when the pipeline is applied to the challenge video. The more obvious one appears when the lanes are significantly curved. The detected lane lines cannot follow the curve by the current pipeline design, and thus simply go straight through the curve and only follow the straight portion of the lane closer to the vehicle. Another shortcoming appears when the lane is difficult to detect against the color of the road. The detected left lane line actually disappears for a split second in this section of the challenge video, likely because the pipeline does not detect any valid edges in that section of the road.


###3. Possible Improvements

A possible improvement would be to perhaps use the detected lane line from the previous few frames when no new lane line is detected. Using historical lane lines would most likely be accurate because the lane is presumably a continuous line. For the segments of time where the lane cannot be detected, an extrapolation based on the previous lane line should suffice. This would address the issue of disappearing lane lines.

Another simple improvement would be to look for lane lines on the appropriate half of the image. As noted above, I looked for positively and negatively sloped lines to determine whether they belonged to the left lane or right lane. This could be further improved by only looking for positively sloped lines on the left half of the image and negatively sloped lines on the right half of the image. Considering lines from the entire region of interest leads to noise that could potentially alter the average slope of the lane line.