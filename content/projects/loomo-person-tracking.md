---
title: "Gesture-Triggered Person Following on a Loomo Robot"
date: 2022-06-01
draft: false
summary: "Perception pipeline for EPFL's human–robot tandem race: a Segway Loomo picks out the person who raises a hand using AlphaPose keypoints, then keeps them locked in with YOLOv5 + DeepSORT while it follows them."
period: "Spring 2022"
affiliation: "EPFL, Deep Learning for Autonomous Vehicles (DLAV)"
role: "Course project"
status: "Completed"
tags: ["Computer vision", "Robotics", "Tracking", "PyTorch"]
featuredImage: ""
links:
  - name: "Code on GitHub"
    url: "https://github.com/danialzendehdel/DLAV_race"
publication: ""
weight: 20
---

## The problem

In the DLAV final race, a Segway Loomo robot has to follow one specific person through a course that other people are walking through. The robot must decide *who* to follow without any manual labelling, and then not lose or swap that person when others cross in front.

## How it works

The pipeline has two stages, switched by a small state machine.

1. **Find the person of interest.** Every frame goes through AlphaPose (ResNet-50 backbone, YOLO person detector). The person of interest is whoever makes the start gesture, one hand raised above the shoulder and the other below it, checked from the shoulder and wrist keypoints with a confidence threshold so half-occluded people are skipped.
2. **Track them.** Once found, the pipeline hands over to a YOLOv5 detector with DeepSORT and an OSNet re-identification model. The person of interest is matched to a DeepSORT track ID by the distance between box centres (an IoU-based matcher is also implemented), and from then on the robot follows that ID, so it holds on through crossings and short occlusions.

The output to the robot's controller is the bounding box of the person to follow.

## Stack

Python, PyTorch, AlphaPose, YOLOv5, DeepSORT with OSNet re-ID, OpenCV. An earlier variant with YOLOv4 (TensorFlow) and DeepSORT is also in the repo.
