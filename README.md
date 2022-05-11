# Implementation of Canny's Edge Detector

Edge detection is very important to find sudden changes in an image. Here you will learn about the implementation of a very important edge detector called Canny’s edge detector.


## Canny-Edge-Detector
There are some basic steps for the implementation of a Canny edge detector.

1. Generation of Masks
2. Applying Masks to Images, Compute gradient magnitude, Compute gradient Direction
3. Non-Maxima Suppression
4. Hysteresis Thresholding

### Steps for implementation:
1.**Generation of Masks:** This module requires the value of sigma as an input and generates x- and y-derivative masks as output.
  1. Mask size: To generate the masks, the first step is a reasonable computation of the mask size. Mask size depends on the value of sigma and T. Formula of size of half mask is as follows:
       sHalf = round(sqrt(-log(T) * 2 * sigma^2))
       
  2. Where sigma and T are input parameters.
       1. Sigma can be 0.5, 1, 1.5... The lower limit of sigma is 0.5.
       2. Width of the mask is based on parameter T. T can be any value between 0 and 1. For example T=0.3. The above formula will give us the size of half mask.
           
  3. The total mask size would then be computed as follows:
       N= 2*sHalf +1 (total mask size)
       
  ### Example:

  sigma=0.5; T=0.3

  sHalf = round(sqrt(-log(T) * 2 * sigma^2)) => 1

  N= 2*sHalf +1 => 3

[Y, X] = np.meshgrid ( (-sHalf : sHalf+1), (-sHalf : sHalf+1) ) => Y= [[-1,0,1] [-1,0,1] [-1,0,1]]

  1.  In Canny's method, the masks used are the 1st derivative of Gaussian in x- and y-directions.
      The gaussian formula is as follows:
      G (x, y) = exp (-(x^2 + y^2)/(2*sigma^2)
  2. Now take the first derivative of Gaussian w.r.t ‘x’ and call it ‘fx’ and put the valve of X, Y in it. Repeat this procedure w.r.t ‘y’ as well. These (Gx, Gy) are the two masks that will be convolved with the image in the next step.
  3. Multiply the Gx and Gy with 255 to scale up and then round off these values so that convolution is performed with integer values rather than floats. Save the scale factor, because gradient magnitude will be later scaled down (after convolution) by the same factor.
  
  
**Applying Masks to Images:** The masks are applied to the images using convolution. Store
convolution results in fx and fy.

**Compute gradient magnitude:** Compute the gradient magnitude of images (fx, fy) at each pixel after convolving with masks (Gx, Gy). M is computed from fx and fy images, using the magnitude formula: 

![\Large M=\sqrt{fx^2 + fy^2}](https://latex.codecogs.com/svg.image?M=&space;\sqrt{&space;fx^2&space;&plus;&space;fy^2}) 

**Compute gradient Direction:** Compute the gradient direction of images (fx, fy) at each pixel after convolving with masks (Gx, Gy). Phi is computed using atan2 function by the following a formula: 

![\Large Theta&space;=&space;arctan&space;\frac{fy}{fx}](https://latex.codecogs.com/svg.image?\Theta&space;=&space;arctan&space;\frac{fy}{fx};)

**Non-Maxima Suppression:** Non-Maxima suppression step makes all edges in M one pixel thick.
  i. The first step is to quantize gradient direction into just four directions.


| Value assigned | Angle| 
| :---:         |     :---:      | 
| 0  | 0 to 22.5, 157.5 to 202.5, 337.5 to 360     | 
| 1   | 22.5 to 67.5, 202.5 to 247.5     | 
| 2  | 67.5 to 112.5, 247.5 to 292.5    | 
| 3  |112.5 to 157.5, 292.5 to 337.5     | 

   ii. The next step is to pick two neighbors of each edge point along the gradient direction. This is
because gradient direction is perpendicular to the edge, and therefore, this is the direction in which
we are searching for edge points.
   iii. Therefore, the two neighbors that need to be picked for comparison are the north and south
neighbors. If the edge point (r, c) is greater than both these neighbors, then it is maintained in M
otherwise it is made zero.

**Hysteresis Thresholding:** The final step in Canny's edge detection algorithm is to apply two
thresholds to follow edges.
   1. First made the border pixels zero, so that finding neighbors does not go out of bounds of the
image.
   2. Next, the image is scanned from left to right, top to bottom. The first pixel in non-maxima
suppressed magnitude image which is above a certain threshold, Th, is declared an edge.
   3. Then all its neighbors are recursively followed, and that above threshold, Tl, is marked as an
edge.
   4. Thus, there are really two stopping conditions:

       i. if a neighbor is below Tl, we won’t recurs on it.
       
       ii. if a neighbor has already been visited, then we won't recurs on it.
