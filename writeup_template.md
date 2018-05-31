## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Before staring the project, I decide to use the YOLOv3 with the project. 
* Optionally, I convert the YOLOv3 to keras model. 
* Note: Because it will spend a lots of time to deal with the entire video,So I split the video into 10 parts and the time is greatly reduced
* Connecting multiple video's together with moviepy

[//]: # (Image References)
[image1]: https://github.com/RedMery/Vehicle-Detection/blob/master/YOLOv3.jpg
[image2]: ./examples/HOG_example.jpg
[image3]: ./examples/sliding_windows.jpg
[image4]: ./examples/sliding_window.jpg
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### You only look once(YOLOv3)

#### 1. Explain how the YOLO works

Prior detection systems repurpose classifiers or localizers to perform detection. They apply the model to an image at multiple locations and scales. High scoring regions of the image are considered detections..  

We use a totally different approach. We apply a single neural network to the full image. This network divides the image into regions and predicts bounding boxes and probabilities for each region. These bounding boxes are weighted by the predicted probabilities.

![alt text](https://raw.githubusercontent.com/RedMery/Vehicle-Detection/master/BBox.png)

The YOLOv3 has several advantages over classifier-based systems. It looks at the whole image at test time so its predictions are informed by global context in the image. It also makes predictions with a single network evaluation unlike systems like R-CNN which require thousands for a single image. This makes it extremely fast, more than 1000x faster than R-CNN and 100x faster than Fast R-CNN. See our paper for more details on the full system.

#### 2. Convert the yolo to keras model

I convert the yolo to keras model with the code:

```
python yad2k.py cfg/yolo.cfg yolov3.weights data/yolo.h5
```
The keras model looks like :

![YOLOv3](https://raw.githubusercontent.com/RedMery/Vehicle-Detection/master/YOLOv3_arch.png)

#### 3. Describe how run the pipeline.

I split the project_video.mp4 into 10 parts with this code:

```
project_video = VideoFileClip(input_video, target_resolution=(720, 1280)).subclip(0, 5)
```
#### 4. The pipeline
The pipeline is like this:

```
input_video = './project_video/project_video_1.mp4'
yolo = YOLO(0.6, 0.5)
file = 'data/coco_classes.txt'
all_classes = get_classes(file)


out_video = 'project_video_output/project_video_output_1.mp4'
project_video = VideoFileClip(input_video, target_resolution=(720, 1280)).subclip(0, 5)
ret_clip = project_video.fl_image(Vehicle_Detection)
%time ret_clip.write_videofile(out_video, audio=False)
```

### Video Implementation

#### Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

When I run the pipeline, it spend a lots of time. even though I split the video into 10 parts, it still take 5 hours. I want to search a eficient way to solve this problem.


