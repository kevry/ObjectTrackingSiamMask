# CS585 Final Project - Object Tracking with SiamMask
Kevin Delgado, Jorge Paolo Casas, Matthew Susanto

## Problem Definition

Object tracking is a core component within the field of computer vision and has many real-world applications such as gesture recognition and security systems. The topic of object tracking can be further broken down into two smaller categories namely 1) visual object tracking and 2) video object segmentation. Visual object tracking involves online estimation of the position of an arbitrarily assigned object throughout the video given the object's position in the first frame. Video object segmentation is similar to visual object tracking except that instead of returning the position of the object, commonly visualized through a axis-aligned or rotated bounding box, it returns a binary segmentation mask. Such a mask contains pixel-level information regarding the contours of the object which provides much more information than a bounding box but often comes at the cost of increased computational resources. The method investigated in this project, SiamMask, provides a simple framework which can, once trained, robostly output rotated bounding boxes and segmentation masks for an object in an online manner after a user-inputted initalization of the object based on the first frame.

## Breakdown of SiamMask Implementation


## Implementing and Testing SiamMask on DAVIS 2017

To test SiamMask ourselves, we followed the tutorial on the main SiamMask github page which can be found [here](https://github.com/foolwood/SiamMask). All processing and tests conducted on Windows 10 (Processor: Intel(R) Core(TM) i7-2600 CPU @ 3.40GHz, 3401 Mhz, 4 Core(s), 8 Logical Processor(s), Memory 16GB). Initially we attempted to train the SiamMask network from scratch ourselves, but due to the nature of the SiamMask architecture and limitations in hardware, we opted to use a pretrained network. The training process for SiamMask involves training the network on COCO, ImageNet-VID, and YouTube-VOS, all of which are substantial datasets. Just attempting to download the COCO dataset gave us an estimated time of 5 hours. The authors state that training take about 10 hours when run on 4 Tesla V100 GPUs. Given that we had no GPUs of similar computing capability available, this training process would've taken a long time. Below are screenshots depicting us setting up SiamMask and running the evaluation script on the DAVIS 2017 dataset.

![screenshot1](https://user-images.githubusercontent.com/50607673/116298004-eaf10200-a769-11eb-8fd9-b508056426ad.png)
The screenshot above depicts us running equivlanet commands outlined in the make.sh file provided by the authors. We decided to run each command one-by-one rather than running everything once as we had a couple issues regarding unsuported modules as the last update to the repository was in 2019.

![screenshot4](https://user-images.githubusercontent.com/50607673/116301863-81bfbd80-a76e-11eb-8e39-3084a08889cb.png)

This screenshot shows the output of running the evaluation script on the DAVIS 2017 trainval dataset. It's worth pointing out that the authors state that this command should take about 50s total but based on the output gathered on our system, the processing the first video alone took 91.3s, further emphasizing the limitations in hardware. 
## Results

### Quantitative Results
Running the provided evaluation script outpus a quantitaive metric called IOU. IOU stands for intersection over union. For image segmentation applications, IOU is also known as  the Jaccard index, a metric which quantifies the percent overlap between the current predicted mask and the target mask. For the DAVIS dataset, each set of frames representing a video is accompanied by an annotated version of the frames which contains the correct position of the mask. In practice, this target mask would be specified by the user and initalized based on the first frame.
### Qualitative Results

https://user-images.githubusercontent.com/50607673/116309067-4cb76900-a776-11eb-9ba3-d28e85525dd3.mp4

https://user-images.githubusercontent.com/50607673/116309224-81c3bb80-a776-11eb-8070-ba47279cee95.mp4

https://user-images.githubusercontent.com/50607673/116308390-6d32f380-a775-11eb-85c9-4301b121dc78.mp4




## Recycling Data
