An <span style="color:#e1db3d">Edge</span> is a region where there is rapid change in image intensity within a small window. A corner is where two edges meet at an <span style="color:#e1db3d">angle</span>.
Causes of edges:
\- <span style="color:#e1db3d">Surface Normal Discontinuity</span> : Same surfaces but their orientation may be different causing them to receive different amounts of light, hence they seem different and make an edge where they meet.
\- <span style="color:#e1db3d">Depth Discontinuity</span> : This is due to different surfaces reflecting different frequencies and intensity of light and there being multiple objects in the scene.
\- <span style="color:#e1db3d">Suface reflectance Discontinuity</span> :
\- <span style="color:#e1db3d">Illumination Discontinuity</span> : Shadows 

### Gradient of an image:
The <span style="color:#e1db3d">maximas</span> and <span style="color:#e1db3d">minimas</span> of the slope of the image(derivative of the intensity function) in one dimension will give exactly the regions where there is change in intensity and taking the absolute values would give us the edges, the magnitude will also tell us the abruptness of the edge.
For a 2D case we can get the partial derivative along each axis and those two vectors are enough to signify the changes along any axis.

This for a discrete image it is simply the change in two consecutive pixels divided by the distances from their centers. This operation can be captured by a convolution.
When we have $2*2$ convolution, we get amazing localization, but susceptible to noise.
As the size of the convolution increases there is a lot more smoothing going on, a larger area is considered so less chance for anomalies to ruin the computation.
Thresholding is done to decide if there is an edge or not, if the gradient is high enough then there is an edge, or there is not.

### Laplacian of an image:
We take the second derivative, here maximas and minimas don't denote edges, but the <span style="color:#e1db3d">zero crossings</span>. Laplacian is the second derivative, i.e. difference of the difference, so we need at least 3 pixels, so a $3*3$ convolution is necessary.![[Pasted image 20240229145717.png]]If there is a lot of noise, the first derivative is not enough to find the edge, this means the noise must be dealt with, so we apply gaussian smoothing and that removes the noise but also smooths the edge a tiny bit. Then the derivative works, this can be done in a single operation by applying the derivative of the gaussian to the image.
### Canny Edge Detector:
This is what takes in the best of both worlds, firstly using the first derivative, i.e the gradient we find the direction of the gradient at every pixel, then we find the laplacian along that direction, this is beneficial because this does not suppress edges in that take sharp quick turns and are in one general direction. This is calculated according to the gradient information of each pixel.  