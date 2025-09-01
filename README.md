# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program:
```
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
faceImage = cv2.imread('easwar.jpg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")

faceImage.shape

faceImage = cv2.resize(faceImage, None, fx=3, fy=3, interpolation=cv2.INTER_LINEAR)

faceImage.shape

# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('sunglass.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")

# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(1000,350))
print("image Dimension ={}".format(glassPNG.shape))

# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]

# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');

# Make a copy
#faceWithGlassesNaive = resized_faceImage.copy()
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region with the sunglass image
faceWithGlassesNaive[900:1250,730:1730 ] = glassBGR
plt.imshow(faceWithGlassesNaive[...,::-1])

# Make the dimensions of the mask same as the input image.
# Since Face Image is a 3-channel image, we create a 3 channel image for the mask
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI= faceWithGlassesArithmetic[900:1250,730:1730]

# Use the mask to create the masked eye region
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

# Use the mask to create the masked sunglass region
maskedGlass = cv2.multiply(glassBGR,glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")

# Replace the eye ROI with the output from the previous section
faceWithGlassesArithmetic[900:1250, 730:1730]=eyeRoiFinal

# Display the final result
plt.figure(figsize=[20,20]);
plt.subplot(121);plt.imshow(faceImage[:,:,::-1]); plt.title("Original Image");
plt.subplot(122);plt.imshow(faceWithGlassesArithmetic[:,:,::-1]);plt.title("With Sunglasses");

```
## Output:

<img width="432" height="591" alt="image" src="https://github.com/user-attachments/assets/62f07b2f-b985-4cff-8985-bc870318ae11" />
<img width="700" height="388" alt="image" src="https://github.com/user-attachments/assets/4040510b-8e7f-4056-9c23-dea1bcba2885" />
<img width="1365" height="252" alt="image" src="https://github.com/user-attachments/assets/9aee113b-788b-44b6-aff5-c5a0f0fb5af3" />
<img width="446" height="557" alt="image" src="https://github.com/user-attachments/assets/06eb50d7-a4c6-4384-a6eb-049e6cde5e9b" />
<img width="1371" height="222" alt="image" src="https://github.com/user-attachments/assets/59c03acf-d42a-4c95-b7be-ea172210508e" />
<img width="1227" height="765" alt="image" src="https://github.com/user-attachments/assets/aa1c7c27-4ec9-46ed-b1aa-2049dfff9177" />

