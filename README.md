# Canny-Edge-Detector
There are some basic steps for the implementation of a Canny edge detector.

1. Generation of Masks
2. Applying Masks to Images, Compute gradient magnitude, Compute gradient Direction
3. Non-Maxima Suppression
4. Hysteresis Thresholding

## Steps for implementation:
1.** Generation of Masks:** This module requires the value of sigma as an input and generates x- and y-derivative masks as output.

       *Mask size: To generate the masks, the first step is a reasonable computation of the mask size. Mask size depends on the value of sigma and T. Formula of size of         half mask is as follows:
       sHalf = round(sqrt(-log(T) * 2 * sigma^2))
       
       *Where sigma and T are input parameters.
           *Sigma can be 0.5, 1, 1.5... The lower limit of sigma is 0.5.
           *Width of the mask is based on parameter T. T can be any value between 0 and 1. For example T=0.3. The above formula will give us the size of half mask.
           
       *The total mask size would then be computed as follows:
       N= 2*sHalf +1 (total mask size)
2. 
