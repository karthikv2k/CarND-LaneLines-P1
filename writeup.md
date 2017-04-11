# **Finding Lane Lines on the Road** 

The aim of the project is to detect lane lines on highways using traditional CV techniques. The images are part of videos taken by vehicles travelling inside a lane. This makes the problem easy to model as there is no need to handle lane switches and merging.

[image1]: ./test_images/solidWhiteCurve.jpg "Lanes with solid white curve"
[grayscaled]: ./output_images/grayscaled.png
[blurred]: ./output_images/blurred.png
[edges]: ./output_images/edges.png
[lanes]: ./output_images/lanes.png
[lanes_debug]: ./output_images/lanes_debug.png
[lines]: ./output_images/lines.png
[lines_superimposed]: ./output_images/lines_superimposed.png
[masked]: ./output_images/masked.png

---

## Example image

![Lanes with solid white curve][image1]

## Solution overview

The solution consists of series of standard image transformations and then some custom calculations to find lane lines,

### Standard image transformations

#### Grayscaling
This is done to remove our dependence on color of the lane markings. This simplies our solution and also makes it robust to lighting changes.

![][grayscaled]

#### Blurring
Blurring an image removes spurious noise during edge detection. 

![][blurred]

#### Edge detection using Canny edge detection algorithm
Here, the parameters for edge detection are tuned for this specific problem.

![][edges]

#### Masking the region of interest
Since the camera position is in middle of the lane, we can mask only the region that we know the lane lines would exist. This would help us in reducing the noise introduced by other lanes in the road.

![][masked]

#### Extracting lines using Hough transformation
Simillar to edge detection, we tuned the parameters specific to this problem.

![][lines]

### Detecting lanes using custom logic 
So far, we have used only standard transformations. But here, we use our custom logic to detect lanes from output of the last stage of the transformed image. The idea is to find which direction the line segments from hough transformation are pointing and finding two common direction. The left lane has a positive slope whereas the right lane has negative slope. This makes the solution little easy by putting the line segments into two groups: positive and negative slope. Then, for each group, we look at the distribution of x values at two y values: bottom of the image (image height) and around 65% of image height. This makes things easy to draw the lane lines and also makes debugging much easier when compared to finding mean slopes in those two groups. Also we removed outliers when calculating mean for x values. Without that, line segments outside of the lane lines would have pulled the mean away from the lane locations. The image below shows the distribution of x values at two y locations for each groups.

![][lines_superimposed]
![][lanes_debug]
![][lanes]


### Potential shortcomings with our solution
1. Doesn't perform well with concrete roads as the contrast between lanes and the road reduces
2. It assumes the vehicle is always in the middle of the road
3. Doesn't do well with curved lanes as basic approach is to fit a line not a curve
4. Whenever there are cars nearby, it introduces lot of line segments that makes finding lane lines difficult

### Possible improvements
1. Account for curved lanes by fitting a curve rather than a line
2. Account for nearby vechiles either by removing line segemnts due to them or making the curve fitting more robust
3. Use a ML based aproach :)

* Unfortunately I couldn't collaborate with others for this project but I used pronoun "we" in the doc because it is easy for me write that way.
