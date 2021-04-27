# CS585 Final Project - Object Tracking with SiamMask
Kevin Delgado, Jorge Paolo Casas, Matthew Susanto

## Problem Definition

Object tracking is a core component within the field of computer vision and has many real-world applications such as gesture recognition and security systems. The topic of object tracking can be further broken down into two smaller categories namely 1) visual object tracking and 2) video object segmentation. Visual object tracking involves online estimation of the position of an arbitrarily assigned object throughout the video given the object's position in the first frame. Video object segmentation is similar to visual object tracking except that instead of returning the position of the object, commonly visualized through a axis-aligned or rotated bounding box, it returns a binary segmentation mask. Such a mask contains pixel-level information regarding the contours of the object which provides much more information than a bounding box but often comes at the cost of increased computational resources. The method investigated in this project, SiamMask, provides a simple framework which can, once trained, robostly output rotated bounding boxes and segmentation masks for an object in an online manner after a user-inputted initalization of the object based on the first frame.

## Breakdown of SiamMask Implementation


## Implementing and Testing SiamMask 


## Results
