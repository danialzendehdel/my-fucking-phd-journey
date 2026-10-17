---
title: "Multi-Objective Battery Dispatching with an Enhanced SAC Algorithm"
date: 2025-06-30
draft: true
summary: "Dispatch policies that balance operating cost against battery degradation, built on an extended Soft Actor-Critic and evaluated against optimisation baselines."
period: "2024 – 2025"
affiliation: "Sapienza Università di Roma"
role: "PhD research"
status: "Published"
tags: ["Reinforcement learning", "Energy systems", "Battery modelling", "Multi-objective"]
links: []   # add {name: "Paper", url: "<DOI link>"} entries here
publication: "Zendehdel, D., De Santis, E., Capillo, A., Odonkor, P., & Rizzi, A. (2025). *Multi-Objective Battery Dispatching using an Enhanced SAC Algorithm.* IJCNN, pp. 1–8, IEEE. — Zendehdel, D., Ferro, G., De Santis, E., & Rizzi, A. (2025). *Degradation-Aware Energy Management in Residential Microgrids: A Reinforcement Learning Framework.* IJCCI, pp. 538–557, Springer Nature Switzerland."
weight: 40
showTableOfContents: false
---

{{< project-meta >}}

## Overview

Cycling a battery hard saves money today and shortens its life tomorrow. A dispatch policy that only minimises operating cost will quietly spend the battery. This project makes degradation an explicit objective and lets the policy trade the two against each other.

## Approach

- Multi-objective reward design in which operating cost and a degradation term are traded against each other with an explicit weight.
- An enhanced Soft Actor-Critic (SAC) algorithm for the continuous dispatch action, implemented in PyTorch.
- Evaluation against optimisation baselines on the same microgrid scenarios.

## Results

The policies reduce battery degradation at comparable operating cost. The work was published twice: as the enhanced SAC algorithm (IJCNN 2025) and as a degradation-aware reinforcement learning framework for residential microgrids (IJCCI 2025, Springer).
