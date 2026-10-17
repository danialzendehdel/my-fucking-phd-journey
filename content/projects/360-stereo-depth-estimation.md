---
title: "Stereo Depth Estimation on a Real, Unrectified 360° Dataset"
date: 2023-03-01
draft: false
summary: "MSc thesis: recorded a new real-world dataset of 17,373 stereo 360° image pairs with LiDAR depth ground truth, and adapted deep stereo networks (PSMNet, 360SD-Net) to work on it without rectification."
period: "2022 – 2023"
affiliation: "Sapienza University of Rome · EPFL VITA Lab"
role: "MSc thesis"
status: "Completed"
tags: ["Computer vision", "Depth estimation", "Datasets", "PyTorch"]
featuredImage: "/images/projects/360-depth-rig.jpg"
featuredImageAlt: "Two vertically stacked 360-degree cameras and an Ouster LiDAR mounted on an autonomous delivery vehicle, with sample upper and lower panoramas and the LiDAR depth image"
featuredImageCaption: "Recording rig: two vertically stacked RICOH Theta 360° cameras above an Ouster OS1-64 LiDAR, with an upper image, lower image and LiDAR depth frame from the same instant."
links:
  - name: "Code & thesis on GitHub"
    url: "https://github.com/danialzendehdel/Thesis"
publication: "MSc thesis, *A Deep Learning Architecture for Enhanced Depth Estimation on an Unrectified Stereo 360° Dataset*, Sapienza University of Rome. Supervisors: Prof. Antonello Rizzi (Sapienza), Prof. Alexandre Alahi (EPFL)."
weight: 10
---

## The problem

Stereo depth networks are trained almost entirely on synthetic data and on rectified pinhole image pairs. Real 360° cameras on a vehicle give neither: the images are heavily distorted equirectangular panoramas, and rectifying them needs careful calibration that does not survive outside the lab. Existing 360° depth datasets (Matterport3D, Stanford 3D) are synthetic or rendered, so there was nothing realistic to train or evaluate on.

## What I built

**A new dataset.** I designed and recorded a real-world dataset with two RICOH Theta 360° cameras stacked vertically above an Ouster OS1-64 LiDAR, at EPFL in Lausanne.

- 17,373 time-synchronised tuples (upper image, lower image, LiDAR depth image and point cloud) across two outdoor street scenes and two indoor corridors.
- 3840 × 1920 stereo images, with LiDAR depth images at 1024 × 64 as ground truth (depth, not disparity).
- Deliberately left **unrectified**, to make the dataset closer to what a deployed system sees. I removed the LiDAR distortion and aligned its panoramic output with the camera format so the three streams line up.

**A network that works on it.** I benchmarked PSMNet and 360SD-Net, then modified 360SD-Net to train on the new data: a feature extractor with residual blocks and dilated convolutions, an ASPP module for context, a cost volume regularised by a stacked-hourglass 3D CNN, and depth regression. The polar angle of each pixel is fed in through a separate branch and fused late, to give the network the distortion pattern of the equirectangular projection. Trained for 500 epochs in PyTorch.

## Results

![Qualitative depth results on an outdoor street scene](/images/projects/360-depth-results.jpg)

- Out of the box, 360SD-Net did not cope with the unrectified pairs (RMSE in the thousands on the Outdoor 1 scene); the modified network reached an RMSE of 0.9 on the same scene.
- Ablations showed larger input crops help (256 × 512 beat 256 × 256), while the polar-angle branch contributed little on this dataset, so it can be dropped to cut compute.
- The dataset, released to the lab, gives a harder and more realistic benchmark for 360° stereo depth than the synthetic alternatives.
