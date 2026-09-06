---
title: "Deep Stereo Depth Estimation on Unrectified 360° Images"
date: 2022-08-01
draft: false
summary: "Deep stereo networks adapted to unrectified 360-degree camera images, plus a new lidar-and-camera dataset collected and released for the group."
period: "Feb 2022 – Aug 2022"
affiliation: "EPFL, VITA Lab"
role: "Visiting Researcher"
status: "Completed"
tags: ["Computer vision", "Depth estimation", "Datasets"]
featuredImage: ""
links: []
publication: ""
weight: 60
---

## Overview

Stereo depth networks assume rectified pinhole image pairs. Two 360-degree cameras on a mobile platform give neither. This project adapted existing deep stereo architectures to unrectified omnidirectional images and built the dataset needed to train and evaluate them.

## Approach

- Reproduced the published benchmark results of existing deep stereo networks on their original datasets before changing anything.
- Modified the network architectures so they operate on unrectified 360-degree image pairs.
- Built a data collection rig from a lidar and two 360-degree cameras, recorded a new dataset, debugged the lidar extraction, and released the cleaned dataset in a standard format for the group.

## Results

Working depth estimation on unrectified 360-degree pairs, and a cleaned dataset released for the group. The work also formed the MSc thesis at Sapienza.
