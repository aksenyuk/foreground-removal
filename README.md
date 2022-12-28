# Image Foreground Removal

Co-authored-by: [@giminosk](https://github.com/Giminosk)

### Note: Download  `own_dataset` manually from `datasets` folder, instead of using `!wget` as it is in the source code.

### The algorithm performed:

- Background/foreground segmentation by image comparison:

  1. Convert to grayscale
  2. Apply Gaussian Blur
  3. Repeat Step (1), (2) for the second image
  4. Calculate absolute difference between images obtained at previous steps
  5. Apply threshold to Step (4)
  6. Apply dilation to Step (5)
  7. Convert Step (6) to BGR to the final image with foreground removed

- Collect background colors from the segmented image:
  
  1. Create mask for black color
  2. Apply this mask to the image
  3. Store the colors from the masked image to a list
  4. Remove black color from the resulting set of colors

- Create "base" image to fill the foreground on:

  1. Combine masks obtained at Point (1) by merging it with the median of the initial image dataset

- Calculate MLE (Maximum Likelihood (Neighborhood) Estimation):

  1. Calculate all the needed metrics:
  
      - Mean
      - Covarience
      - Inverse of Covarience
      - Probability of a pixel (difference between image pixels and mean calculated in Step (1))
      - Dot Product of probability matrix and inverted covarience matrix
      - Exponential of einsum of probability matrix and the result of the previous Step
     
  2. Store to a list MLE of each pixel of each image

- Fill the foreground with pixels of probability higher than 0.15 threshold from the initial dataset images

- Fill the remained foreground pixels with ones from the initial dataset images the colors of which are also in the set of background colors collected in Point (2)

- Apply averaging of neighboring pixels to the rest of the pixels left blank

  (Note: Filling these pixels simply with the ones from the initial dataset images is also worth trying)
