# CS585 Final Project - Object Tracking with SiamMask
Kevin Delgado, Jorge Paolo Casas, Matthew Susanto

## Problem Definition

Object tracking is a core component within the field of computer vision and has many real-world applications such as gesture recognition and security systems. The topic of object tracking can be further broken down into two smaller categories namely 1) visual object tracking and 2) video object segmentation. Visual object tracking involves online estimation of the position of an arbitrarily assigned object throughout the video given the object's position in the first frame. Video object segmentation is similar to visual object tracking except that instead of returning the position of the object, commonly visualized through a axis-aligned or rotated bounding box, it returns a binary segmentation mask. Such a mask contains pixel-level information regarding the contours of the object which provides much more information than a bounding box but often comes at the cost of increased computational resources. The method investigated in this project, SiamMask, provides a simple framework which can, once trained, robostly output rotated bounding boxes and segmentation masks for an object in an online manner after a user-inputted initalization of the object based on the first frame.

## Breakdown of SiamMask Implementation
SiamMask is an object detection tool that performs visual object tracking among with semi-supervised video object segmentation. The model relies on a single bounding-box initialization and operates online, producing class-agnostic object segmentation masks and rotated bounding boxes during the length of the process. SiamMask consists of two variants, three-branch architecture and two-branch architecture both using ResNet50 as the pre-built model excluding the last convolutional layer in the 4-th stage of the CNN. To increase the receptive field, dilated convolutions, were used instead.

The backbone of the network for SiamMask was trained on Imagenet-1K. For optimization Stochastic Gradient Descent was used. The intial epoch consists of a learning rate of 10<sup>-3</sup> and then linearly increasing to 5*10<sup>-3</sup>. After, the learning rate increases logarithmically for 15 more epochs. The remaining network was trained using COCO, ImageNet-VID, and YouTube-VOS
<img width="859" alt="siammask_nn" src="https://user-images.githubusercontent.com/45439265/116323390-e0dffb00-a78b-11eb-8755-ab8b56f4048d.png">

## Implementing and Testing SiamMask on DAVIS 2017
To test SiamMask ourselves, we followed the tutorial on the main SiamMask github page which can be found [here](https://github.com/foolwood/SiamMask). All processing and tests conducted on Windows 10 (Processor: Intel(R) Core(TM) i7-2600 CPU @ 3.40GHz, 3401 Mhz, 4 Core(s), 8 Logical Processor(s), Memory 16GB). Initially we attempted to train the SiamMask network from scratch ourselves, but due to the nature of the SiamMask architecture and limitations in hardware, we opted to use a pretrained network. The training process for SiamMask involves training the network on COCO, ImageNet-VID, and YouTube-VOS, all of which are substantial datasets. Just attempting to download the COCO dataset gave us an estimated time of 5 hours. The authors state that training take about 10 hours when run on 4 Tesla V100 GPUs. Given that we had no GPUs of similar computing capability available, this training process would've taken a long time. Below are screenshots depicting us setting up SiamMask and running the evaluation script on the DAVIS 2017 dataset.

![screenshot1](https://user-images.githubusercontent.com/50607673/116298004-eaf10200-a769-11eb-8fd9-b508056426ad.png)
The screenshot above depicts us running equivlanet commands outlined in the make.sh file provided by the authors. We decided to run each command one-by-one rather than running everything once as we had a couple issues regarding unsuported modules as the last update to the repository was in 2019.

![screenshot4](https://user-images.githubusercontent.com/50607673/116301863-81bfbd80-a76e-11eb-8e39-3084a08889cb.png)

This screenshot shows the output of running the evaluation script on the DAVIS 2017 trainval dataset. It's worth pointing out that the authors state that this command should take about 50s total but based on the output gathered on our system, processing the first video alone took 91.3s, further emphasizing the limitations in hardware. 
## Results

### Quantitative Results
Running the provided evaluation script outputs a quantitaive metric called IOU for various segementation thresholds. IOU stands for intersection over union. For image segmentation applications, IOU is also known as  the Jaccard index, a metric which quantifies the percent overlap between the current predicted mask and the target mask. For the DAVIS dataset, each set of frames representing a video is accompanied by an annotated version of the frames which contains the correct position of the mask. In practice, this target mask would be specified by the user and initalized based on the first frame. 

Below is a table outlining the mean IOU, max IOU, the threshold value corresponding to the max IOU, and frames per second for the first 12 videos in the training validation DAVIS 2017 dataset.

| Video Title      | Mean IOU        | Max IOU     | Frames Per Second |
| :----:           |   :----:    |     :----:  |      :----:    |
| Bear             | 0.877       | 0.886       |      1.4     |
| Bike Packing     | 0.457       | 0.553    |      1.6   |
| Black Swan       | 0.830       | 0.838 |     1.7    |
| BMX Bumps        | 0.311       | 0.449   |     1.6    |
| BMX Trees        | 0.469       | 0.628 |      1.6   |
| Boat             | 0.439       | 0.457    |      1.6    |
| Boxing-Fisheye   | 0.451       | 0.659 |      1.4    |
| Breakdance       | 0.372       | 0.381   |      1.6    |
| BreakDance-Flare | 0.656       | 0.661 |      1.6   |
| Bus              | 0.766       |0.769   |      1.5    |
| Camel            | 0.773       | 0.788 |      1.6    |
| Car-Roundabout   | 0.923       | 0.929    |      1.6    |

The following table is the mean IOU and frames per second over all 90 videos in the DAVIS dataset. The full text file containing detailed results can be found [here](DAVIS2017_results.txt).

|         | Mean IOU     | Frames Per Second |
| :----:  |   :----: |  :----: |
| SiamMask|  0.508  |     1.47     |

The authors reported speeds of 55 FPS which is a far cry from the 1.47 FPS achieved on our system, indicating the potential for better results given the proper hardware upgrades.
### Qualitative Results

https://user-images.githubusercontent.com/50607673/116309067-4cb76900-a776-11eb-9ba3-d28e85525dd3.mp4

https://user-images.githubusercontent.com/50607673/116309224-81c3bb80-a776-11eb-8070-ba47279cee95.mp4

https://user-images.githubusercontent.com/50607673/116308390-6d32f380-a775-11eb-85c9-4301b121dc78.mp4

As you can see in the car and swan video, the tracker works pretty well for the user specified object. However, for the judo video, we purposely selected an initial bounding box that would be challenging for the tracker to see how it reacts. You can see that once the person on the left gets flipped over, the tracker only selects the right person. This is likely due to the fact that during the flipping process the two persons who are dressed very similarly were in close proximity with each other which confused the tracker. 


## Recycling Data
As a challenge, we attempted to use SiamMask on the provided recycling dataset. This test was done using a GPU on Google Colab which is why the final result shown below, has much better FPS than the videos displayed above. To apply SiamMask on the recyling data we first connected each frame from the dataset (EXCLUDING the segmented frame), to create a video file for testing. To begin the process, we initalized the bounding box from the annotated first frame which was provided to us. This is the object that will be tracked during the remainder of the process. We used OpenCV2 to create a bounding box as follows:

- Make sure segmented image is grayscale -> cv2.BGR2GRAYSCALE
- Implement a threshold where 7 is the threshold value and 255 is the max value -> cv2.threshold(img, 7, 255,0)
- Next, we find the contours of the image. The largest contour will be the segmented object -> cv2.findContours(thres_output, cv2.RETR_EXTERNAL , cv2.CHAIN_APPROX_SIMPLE
- Finally, we find the bounding rectangle on the largest contour to find the x coordinate, y coordinate, width, and height of the bounding box

These 4 parameters (x, y, width, height) were enough to initiaize the SiamMask tracker and begin tracking the object. During each frame of the video, the SiamMask tracker state was updated, a mask was included on top of the tracked object, with a bounding box included. This iteration was done on all frames of the input recyling video.

#### Below you will see an output video consisting of the modified frames using SiamMask
https://user-images.githubusercontent.com/45439265/116322149-729a3900-a789-11eb-8360-06fc0aaaf56c.mp4

The only difference between what we did for the recycling data and the examples shown above is that there was no user input. As you can see in the video, since the object is partially off-screen in the first frame, the tracker actually switches to a different object. However, once the switch occurs, the tracking works fairly well considering all of the other objects on the conveyer belt and the quality of the video itself.

## Strengths and Weaknesses
SIAMMask proved to be a good technique to track and segment objects in real time. One of the main strengths of SIAMMask is its class agnostic training. By being class agnostic, SIAMMask does not find for objects that it is trained on. Instead, it learns to match pairs of its target to other frames in the video. This means that we wouldn't need to retrain the model when a different video is given. This strength was seen in the different datasets we tried on. Since the model was trained on the COCO dataset, when tested on the DAVIS dataset, the results came out as accurate and clean segmentation and tracking.

However, one problem that we found was when we applied SIAMMask to the recycling dataset. The object that we tracked was located on the top left of the image. In fact, the target object was not fully in frame. Since SIAMMask is trained to recognize objects by matching them to other frames of the video, it was difficult to find a good track and match the target object. From the results video, we see that the track switches from the big plastic bag to the smaller object and tracked this object flawlessly in other frames.
