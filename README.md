# CSE5914 - Docker Tutorial

Please see the [live site](https://evankozliner.github.io/tech-team/) for the actual tutorial.

This repository contains two directories with essentially the same tutorial inside each of them. The "docs" directory contains the version of the tutorial for use by GitHub Pages, whereas the "tutorial-build" directory contains the version of the tutorial appropriate for being built with Docker.

# To Start Working

```bash
gem install jekyll bundler
cd tutorial
bundle exec jekyll serve

```

# Creating the Docker Container
```bash
cd tutorial-build
docker build -t tutorial .

```

# Running the Docker Container
```bash
cd tutorial-build
docker run -p 4000:4000 tutorial

```

# Stopping and Removing All Docker Containers
* Open a new terminal window.

```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

```
* Note: there is no need to re-build the image after this.

