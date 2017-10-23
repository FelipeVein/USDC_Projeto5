**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./imagens/imagem1.jpg
[image2]: ./output_images/test1.jpg
[image3]: ./output_images/test3.jpg
[image4]: ./output_images/test5.jpg
[image5]: ./imagens/imagem5.JPG
[video1]: ./project_video_output.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.



The code for this step is contained in the first four code cells of the IPython notebook.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

Then, I tried several different color_spaces to extract HOG features, and I found out that 'YCrCb' works better than the others.

To extract the hog features, I just used the function "get_hog_features()", presented in the first code cell. This function is exactly the same as the one shown in the course.

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried several different combinations of parameters iteratively and the algorithm returned that the following ones were the best:

* color_space = 'YCrCb'
* orient = 9 
* pix_per_cell = 8 
* cell_per_block = 2
* hog_channel = "ALL" 

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM (fourth code cell) using all of the following three features:

* spatial_feat = True 
* hist_feat = True 
* hog_feat = True 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The sliding window search algorithm can be seen in the fifth code cell. I decided both the scales to search and the overlap by trial and error. With a small overlap, the pipeline does not work, mainly because of the heatmap. 
I chose three different scales to search (also in the fifth code cell): 
* Sliding window x limits
    sw_x_limits = [
    [None, None],
    [None, None],
    [40, image.shape[0] - 40]
    ]
* Sliding window y limits
    sw_y_limits = [
    [400, 640],
    [400, 600],
    [350, 600]
    ]
* Sliding window sizes
    sw_window_size = [
    (120, 120),
    (100, 100),
    (80, 80)
    ]
* Sliding window overlaps
    sw_overlap = [
    (0.5, 0.5),
    (0.5, 0.5),
    (0.5, 0.5)
    ]


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Here are some example images:

![alt text][image2]
![alt text][image3]
![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. After that, I used heatmap to dismiss false positives.

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()`:

### Here are the six test images, their corresponding heatmaps and `scipy.ndimage.measurements.label()`:

![alt text][image5]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The problem with my approach is that it is taking a long time to process each frame of the video. Maybe because I have chosen a greater region of interest than needed. 

I also tried to use a neural network, but it had a worse accuracy on the testing set; maybe because there are fewer images in the training set than needed.


