---
title: "Ship Classification and Localisation"
date: 2022-06-01
draft: false
summary: "Classify ships into five types (cargo, carrier, cruise, military, tanker) and localise them in the image, on ~6,200 hand-annotated images with bounding boxes. Compares a single multi-output MobileNetV2 against a split Xception + MobileNetV2 pair."
period: "MSc coursework"
affiliation: "Sapienza University of Rome, Computational Intelligence (Prof. Antonello Rizzi)"
role: "Co-author, with Peyman Javaheri"
status: "Completed"
featureimagecaption: "Output of the demo notebook on nine test images: predicted and annotated boxes with their IoU, and the predicted class for each ship."
tags: ["Computer vision", "Object detection", "TensorFlow"]
links:
  - name: "Code & notebooks on GitHub"
    url: "https://github.com/danialzendehdel/Ship-Classification-and-Localisation"
publication: ""
weight: 30
showTableOfContents: false
---

{{< project-meta >}}

## The problem

The Game of Deep Learning ship dataset comes with class labels only. The goal here was both to say *what* kind of ship is in an image and *where* it is, so the localisation labels had to be created first.

## What we did

- **Annotation.** Drew bounding boxes for 6,182 images with LabelImg (2,125 cargo, 916 carrier, 776 cruise, 1,148 military, 1,217 tanker), then converted the XML annotations to CSV for training. The annotated CSVs are in the repo.
- **Model 1, multi-output.** One MobileNetV2 backbone with two heads: a softmax classifier over the five classes and a regression head for the bounding box. Trained jointly from a custom generator yielding images, labels and boxes.
- **Model 2, split.** A dedicated Xception classifier plus a separate MobileNetV2 box regressor, each trained on its own task with augmentation (flips, rotations).
- **Evaluation.** Classification accuracy for the label, IoU between predicted and true boxes for localisation, and a demo notebook that loads the trained weights and draws predictions and box centroids on test images. On the nine demo images shown above, every ship is assigned the correct class and box IoU ranges from 56% to 88%.

## Stack

Python, TensorFlow/Keras, MobileNetV2, Xception, LabelImg, Google Colab.
