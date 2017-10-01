# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, the goal is to write a software pipeline to detect vehicles in a video (project_video.mp4), but the main output or product I have created is a detailed writeup of the project.  Check out the [writeup](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup.md) for this project.  

The writeup:
---
The writeup includes the rubric points as well as my description of how I addressed each point.  I have included a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  I have included images in my writeup to demonstrate how my code works with examples.  

The Project
---

The steps in this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Normalize the features and randomize a selection for training and testing.
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Run the pipeline on a video stream (project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

Here are links to the labeled data for [vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) and [non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) examples to train your classifier.  These example images come from a combination of the [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html), the [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/), and examples extracted from the project video itself.   I have also used the recently released [Udacity labeled dataset](https://github.com/udacity/self-driving-car/tree/master/annotations) to augment my training data.  

Some example images for testing the pipeline on single frames are located in the `test_images` folder. The video called `project_video.mp4` is the video my pipeline has demonstrated good work on.  


