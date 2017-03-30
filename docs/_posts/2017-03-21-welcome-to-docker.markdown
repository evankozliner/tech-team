---
layout: post
title:  "What is Docker?"
date:   2017-03-21 18:40:19 -0400
categories: jekyll update
---

## About this tutorial

![docker-whale]({{site.baseurl}}/assets/docker-whale.png)

Welcome to our tech team's tutorial!


This blog is on the basics of using [Docker][docker-url], by the end of the tutorial you should have deployed a copy of this website, (which you can find on our [Github][github-url]) via Docker.

Click [here][tutorial-url] to go to the tutorial.

## What is Docker?

Docker is a containerization platform which means it packages apps and handles their dependencies in an OS-independent fashion. Docker containers are often compared to a virtual machines because both solutions allow developers to host applications or services, however there are key differences between the two. Once installed, Docker will tailors an application's installation to the host operating system, Docker also allows for more computing density than virtual machines because multiple containers can run on a single virtual machine instance sharing the same kernel.

![docker-whale]({{site.baseurl}}/assets/container.png)

## Portability
Docker isolates an applications code from its environment. Details such as networking and storage are abstracted away, this helps guarantee that an app will work when deployed or downloaded on another developers machine. 

## Compute Density
You may be able to run six to eight times as many containers as virtual machines on the same hardware [according to infoworld][infoworld-url]. This can be appealing for high-scale environments who wish to save on hardware. 

## Single Process
* Containers must be a single process so applications that run subprocesses will need to break their subprocesses into seperate containers for Docker to work. Oftentimes this is an easy fix and allows for more precise updates. This could save a developer from needing to shut off a database in order to update a web server.


[docker-url]: https://www.docker.com/
[github-url]: https://github.com/evankozliner/tech-team
[tutorial-url]: https://github.com/evankozliner/tech-team/tutorial.html
[infoworld-url]:http://www.infoworld.com/article/3072929/linux/containers-101-linux-containers-and-docker-explained.html
