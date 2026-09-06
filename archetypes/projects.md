---
# ── Project page front matter ─────────────────────────────────────────────
# Create a new project with:   hugo new projects/<slug>.md
# then edit this file, set draft: false, and commit.
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: {{ .Date }}
draft: true

# One or two sentences shown on the project card. Keep it under ~40 words.
summary: ""

# Human-readable time span and where the work was done.
period: "2026"
affiliation: ""                 # e.g. "Honda Research Institute Europe"
role: ""                        # e.g. "Lead developer", "PhD research"
status: ""                      # e.g. "Ongoing", "Published", "Completed"

# Tags drive the filter buttons on /projects/ and the /tags/ pages.
tags: []                        # e.g. ["Reinforcement learning", "Energy systems"]

# Optional images. Put files under static/images/projects/ and reference them
# with a leading slash, e.g. "/images/projects/my-project.png".
featuredImage: ""
featuredImageAlt: ""
featuredImageCaption: ""

# Links shown on the card and at the top of the project page.
links: []
#  - name: "Paper"
#    url: "https://..."
#  - name: "Code"
#    url: "https://github.com/danialzendehdel/..."

# Optional citation rendered at the bottom of the page (markdown allowed).
publication: ""

# Lower weight = higher on the /projects/ list. Leave 0 to sort by date.
weight: 0
---

## Overview

What the project is, why it matters, and what you did.

## Approach

Method, tooling, key design decisions.

## Results

What came out of it — numbers, plots, a demo.
