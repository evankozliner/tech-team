# CSE 5914 - Docker Tutorial

## What is Docker?


## Installing Docker


## Using Docker to Create & Use Containers

### Creating an Example Application
#### Application Description
For the purposes of this tutorial, we will be using Docker to create an image of this tutorial site. This site is based on Jekyll, so it has several dependencies, including the Ruby programming language, the Bundler gem, and the Jekyll gem. We will not, however, be downloading these dependencies, since that will be the job of the Docker container.

#### Downloading the Application
To download the code for this site, move to an appropriate working directory and execute the following command.

```bash
git clone https://github.com/evankozliner/tech-team.git
```

### Building the Application into an Image
#### Terminology
* Image: a filesystem and set of parameters to be used when running your application or program.
* Container: aninstance of an image that is currently running.

#### Making the Dockerfile
##### Dockerfile Basics
A Dockerfile gives Docker instructions on how to build an image. The basic structure of a Dockerfile is several lines of instructions (written in all capital letters) and their arguments, as shown below.

```bash
INSTRUCTION arguments
```

The basic instructions we will need for this tutorial are explained below.

* FROM: The FROM instruction lets you build your image on top of an image someone has already created, like an image that already contains a specific operating system.
* RUN: The RUN instruction tells Docker to execute a command when setting up a container. This can be used to download dependencies into the container before starting an application.
* CMD: The CMD command actually starts running the application or program that the container was built to run. This is the final instruction executed to set up the container.

##### Creating the Dockerfile for the Example Application
The Dockerfile for an image must be named "Dockerfile" and must be placed in the directory that will be built into an image.

First, navigate to the top level directory of the tutorial site and create a file named "Dockerfile", as shown below.

```bash
cd tutorial-build
emacs Dockerfile &
```

Next, we will need to add the content of the Dockerfile. For this tutorial, we will be building our image off of an existing image with the Ruby language already installed. To do this, add the following line to the Dockerfile.

```bash
FROM ruby:2.3-onbuild
```

The next step for setting up the Docker container will be to download the other dependencies required by this tutorial site. A Ruby gem called Bundler allows Ruby applications to be distributed with a Gemfile that lists all of the Ruby dependencies of that application. This tutorial site comes with such a Gemfile, so we will be using Bundler to install all necessary Ruby dependencies. Add the following line to the Dockerfile.

```bash
RUN bundle install
```

The line shown above will tell Docker it is necessary to execute the command "bundle install" to set up the container. This command will use the Bundler gem to install all of the Ruby requirements that have been specificed in the Gemfile, like Jekyll.

Once this command has been run, all of the dependencies for the site (Ruby, Bundler, Jekyll, etc.) will be in the container. It is now time to tell Docker to start running the Web server to serve this tutorial site. To do this, add the following line to the Dockerfile.

```bash
CMD bundle exec jekyll serve --port 4000 --host 0.0.0.0
```

The line shown above will tell Docker that the final part of setting up the container is to start running the Web server, which for Jekyll is done using the Bundler gem. Here we have specified to start serving the site on port 4000 at IP address 0.0.0.0. This IP address is used to make the site available from outside the container, since it would otherwise default to serving the site on the localhost, which would only be available from within the container.

That's it! The completed Dockerfile should look like what is shown below.

```bash
FROM ruby:2.3-onbuild
RUN bundle install
CMD bundle exec jekyll serve --port 4000 --host 0.0.0.0
```

#### Building the Container

### Running the Container
#### Starting the Container
#### Accessing the Application

### Terminating the Application in the Container
#### Stoppping the Container
#### Removing the Container

## Conclusion
