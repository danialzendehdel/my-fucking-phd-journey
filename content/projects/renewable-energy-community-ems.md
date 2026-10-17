---
title: "Real-Time EMS for a Renewable Energy Community"
date: 2024-06-30
draft: true
summary: "An extended battery equivalent-circuit model used as the simulation environment for a real-time energy management system, capturing behaviour that idealised battery models miss."
period: "2023 – 2024"
affiliation: "Sapienza Università di Roma"
role: "PhD research"
status: "Published"
tags: ["Reinforcement learning", "Energy systems", "Battery modelling"]
featuredImage: ""
links: []   # add {name: "Paper", url: "<DOI link>"} entries here
publication: "Zendehdel, D., et al. (2024). *An Extended Battery Equivalent Circuit Model for an Energy Community Real-Time EMS.* IJCNN, IEEE."
weight: 30
---

## Overview

A renewable energy community shares photovoltaic generation and a battery among several households. The energy management system (EMS) decides in real time when to charge and discharge the battery. Most EMS studies use an idealised battery, which makes learned policies look better than they are once deployed on a real cell. This project builds a more faithful battery model and uses it as the environment the EMS is trained and evaluated in.

## Approach

- An extended equivalent-circuit model of the battery that captures voltage, internal resistance and efficiency behaviour that idealised models miss.
- The model wrapped as a reinforcement learning environment for a real-time EMS.
- Deep RL agents trained for battery dispatch in both continuous and discrete action spaces, and compared with forecasting-based and optimisation baselines.

## Results

The RL dispatch policies achieved lower operating cost than forecasting-based and optimisation baselines while acting on the more realistic battery model. Published at IJCNN 2024.
