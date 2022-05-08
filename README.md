# Canny-Edge-Detector
There are some basic steps for the implementation of a Canny edge detector.

1. Generation of Masks
2. Applying Masks to Images, Compute gradient magnitude, Compute gradient Direction
3. Non-Maxima Suppression
4. Hysteresis Thresholding

## Steps for implementation:
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
  
  
2. **Applying Masks to Images:** The masks are applied to the images using convolution. Store
convolution results in fx and fy.

3. Compute gradient magnitude: Compute the gradient magnitude of images (fx, fy) at each pixel after convolving with masks (Gx, Gy). M is computed from fx and fy images, using the magnitude formula:
