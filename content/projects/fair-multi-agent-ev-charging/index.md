---
title: "Fair Multi-Agent EV Charging under a Shared Power Budget"
date: 2026-06-01
draft: true
summary: "Multi-agent deep RL policies that schedule electric-vehicle charging under a shared power budget, reaching a wider efficiency–fairness Pareto front than the group's evolutionary schedulers."
period: "Jun 2026 – present"
affiliation: "Honda Research Institute Europe"
role: "Research Engineer"
status: "Ongoing"
tags: ["Reinforcement learning", "Multi-agent", "Energy systems"]
links: []
publication: ""
weight: 10
showTableOfContents: false
---

{{< project-meta >}}

## Overview

A fleet of electric vehicles has to charge from a site whose total power is capped. Every vehicle wants to leave with a full battery, but the budget cannot serve all of them at once, so any scheduler has to decide who waits. The usual objective, total energy delivered, hides how that waiting is distributed across vehicles. This project treats the problem as a multi-agent sequential decision problem in which efficiency and fairness are both first-class objectives.

## Approach

- Multi-agent deep reinforcement learning policies, one per charging point, trained under a shared power constraint.
- A simulation environment, cost model and evaluation pipeline built to match the group's published benchmarking protocol, so results are directly comparable to optimisation-based and evolutionary (genetic programming) baselines.
- Constraint-based safety so that learned policies never exceed the site budget, and a reproducible evaluation procedure so results can be re-run from a fixed seed set.

## Results

The learned policies reach a wider Pareto front on efficiency and fairness than the group's genetic-programming schedulers: for a given level of energy delivered they distribute waiting time more evenly, and for a given fairness level they deliver more energy.
