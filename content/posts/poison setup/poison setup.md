+++
date = '2024-11-02T17:21:07+01:00'
draft = false
title = 'Poison setup'
+++

# Poison Theme Manual for Hugo

This guide is intended for users who are new to the Poison theme but have access to the Poison repository, Hugo, and GitHub Actions for deployment. This manual addresses key setup steps, file structure, deployment, and troubleshooting techniques.

---

### Table of Contents
1. [Installing and Running Poison Locally](#installing-and-running-poison-locally)
2. [Deploying to GitHub Pages with GitHub Actions](#deploying-to-github-pages-with-github-actions)
3. [Image Handling and Path Management](#image-handling-and-path-management)
4. [File System Structure](#file-system-structure)
5. [Troubleshooting Common Issues](#troubleshooting-common-issues)
6. [Creating New Posts](#creating-new-posts)
7. [Hugo.toml File Specifications](#hugo-toml-file-specifications)

---
{{< relimg "posts/poison_setup/images/poison_manual.jpg" "manual" >}}

## 1. Installing and Running Poison Locally
1. Clone the repository with the Poison theme submodule.
2. Run the following command in the terminal to fetch submodules:
   ```bash
   git submodule update --init --recursive
   ```
3. Start the Hugo server for local development:
   ```bash
   hugo server
   ```
   
## 2. Deploying to GitHub Pages with GitHub Actions
Here's the basic GitHub Actions setup in `.github/workflows/deploy.yml` to deploy on GitHub Pages:
```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.134.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
## 3. Image Handling and Path Management
- Store images in `static/images` or `assets/images`.
- Reference images in Markdown using `relURL` for consistent pathing:
   ```markdown
   ![Description](&#123;&#123;&lt; relURL "images/example.jpg" &gt;&#125;&#125;)
  ```



## 4. File System Structure
- `content`: Stores pages and posts. Organized into folders like `posts`, `projects`, etc.
- `static`: Contains assets like images, available site-wide.
- `themes/custom-theme`: Custom theme files.


## 5. Troubleshooting Common Issues
### Issue with Relative Paths
- Avoid leading slashes `/` in paths, e.g., use `images/image.jpg` instead of `/images/image.jpg`.

### Sidebar Image Not Showing
- Check the path in `hugo.toml`, using `brand_image = "images/brand_image.jpg"`.

## 6. Creating New Posts
Use Hugo to create a post with front matter automatically:
```bash
hugo new posts/new-post.md
```
The front matter template will look like:
```yaml
---
title: "New Post Title"
date: {datetime.now().strftime('%Y-%m-%dT%H:%M:%S%z')}
draft: false
---
```

## 7. Hugo.toml File Specifications
```toml
baseURL = "https://username.github.io/repo/"
languageCode = "en-us"
theme = "poison"
[params]
    brand_image = "images/brand_image.jpg"
    description = "Site description here"
    [params.menu]
        {Name = "About", URL = "about/", HasChildren = false},
        {Name = "Posts", URL = "posts/", Pre = "Recent", HasChildren = true, Limit = 5},
```

