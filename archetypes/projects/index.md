---
# New project:  hugo new projects/<slug>/index.md
# Put a picture named feature.jpg (or .png) in the same folder: it becomes the card image and page header.
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: {{ .Date }}
draft: true
summary: ""                    # one or two sentences for the card, under ~40 words
period: ""                     # e.g. "2025 – 2026"
affiliation: ""                # e.g. "Sapienza University of Rome"
role: ""                       # e.g. "PhD research"
status: ""                     # e.g. "Completed", "Ongoing", "Published"
tags: []                       # e.g. ["Reinforcement learning", "Energy systems"]
featureimagecaption: ""        # optional caption under the header image
links: []
#  - name: "Code on GitHub"
#    url: "https://github.com/danialzendehdel/..."
#  - name: "Paper (DOI)"
#    url: "https://doi.org/..."
publication: ""                # optional citation, markdown allowed
weight: 50                     # lower = higher on /projects/
showTableOfContents: false
---

{{</* project-meta */>}}

## The problem

## What I built

## Results
