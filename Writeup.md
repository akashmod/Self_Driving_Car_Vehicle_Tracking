##Writeup Template

---

**Vehicle Detection Project**

The steps in this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Apply a color transform and append binned color features, as well as histograms of color, to the HOG feature vector. 
* Note: for those first two steps it is important to normalize the features and randomize a selection for training and testing.
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Run the pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car.jpg
[image2]: ./examples/notcar.jpg
[image3]: ./examples/hog_image.jpg
[video1]: ./final_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

### Histogram of Oriented Gradients (HOG)

#### 1. HOG features extraction from the training images

The code for this step is contained in the second code cell of the IPython notebook. I have defined a function called get_hog_features the purpose of which is to extract the hog features. The hog feature extraction is performed using the method hog from the python library skimage.feature. The method takes in the image, the number of orientations, pixels per cell, cells per block and other information and returns the hog feature vector of the image.

Then I started reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes respectively:

![alt text][image1]
![alt text][image2]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `RGB` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image3]

#### 2. HOG parameters used

I tried various combinations of parameters and settled on 9 orientations, pixels_per_cell=8 and cells_per_block=2. I arrived on these parameters through hit and trial method. I tried various combinations of these and chose the optimum combination when I observed significant accuracy in the vehicle detection.

#### 3. Training of the Classifier

I trained a linear SVM using the method LinearSVC. I first normalized the features vector which consisted of the HOG features and the color features using the method StandardScaler() from sklearn.preprocessing library. I then shuffled and split the data into training and test data and trained the SVM using the normalized training data. The training operation was done in cell 4 of the ipython notebook ["Vehicle_Tracking_P5"](./Vehicle_Tracking_P5.ipynb).

### Sliding Window Search

#### 1. Sliding Window Search Algorithm

I have defined the sliding window algorithm in the function "slide_window" in the cell 2 of the ipython notebook [Vehicle_Tracking_P5](./Vehicle_Tracking_P5.ipynb). When using the sliding window, I used windows of dimensions 96 and 112 with an overlap of 50%, given that the two vehicles to be identified in the video were of dimensions close to this and windows with bigger dimensions helped prevent false positives. This optimum settings for the slide_window was decided based on the results from various combination of the parameters by hit and trial method.


#### 2. Optimization of the performance of classifier

As described before, I used two scales of the windows, 96x96 and 112x112, to slide along the images to search for vehicles. I used RGB 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.
---

### Video Implementation

#### 1. The link to the final output video
Here's a [link to my video result](./final_video.mp4)


#### 2. Filtering of false positives and combining overlapping boxes

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap. I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

However, there were a lot of false positives in the final video to be taken care of. I used the historical data of heatmap to remove the false positives. The idea was to take in data from at least 10 previous frames and put a high threshold for the heat, like 25. This is how I took advantage of using the previous frames as the car in the next fram would be somewhere around the previous frames whereas false positives appear and disappear in random frames. The bounding boxes are then drawn over images at areas with heat values above 25.

### Discussion

#### Possible issues in the pipeline

The major issue that I faced on implementation were the false positives. I used heat values from at least 10 previous frames and put up a high threshold to makes sure that only vehicles are detected and random boxes are not drawn on the frames. 
However, I believe there is more improvement that could be done on the features detection. Having the machine learning algorithm trained with accuracy of 97%, I believe there should not have been false positives in the first place. Further tweaking of the features to be extracted to train the machine learning pipeline can help prevent detection of false positives. The pipeline could fail in occasions where the heat values for the car is not enough for it to be identified or if the false positive remains for several frames enough to add up to the heat value of 25.
