# CSE 5914 - Docker Tutorial

## What is Docker?
Docker is a containerization service. It puts your software into 
independent, lightweight, cross-platform containers that run your program. These containers are run like virtual machines by Docker itself. Thus, a program created on one system can be put into a Docker container that can in turn be run on any machine with Docker installed. This makes it possible to distribute complex applications in a platform-independent manner.

## Installing Docker
1. Update your package lists by running the following command.

    ```bash
    sudo apt-get update
    ```

2. Install the linux-image-extra-* packages using the command shown below.

    ```bash
    sudo apt-get install \
        linux-image-extra-$(uname -r) \
        linux-image-extra-virtual
    ```

3. Add some packages to enable downloading Docker securely.

    ```bash
    sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    ```

4. Add the GPG key for Docker.

    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

5. Add the repository for the latest stable version of Docker.

    ```bash
    sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
    ```

6. Install Docker CE with the command shown below.

    ```bash
    sudo apt-get install docker-ce
    ```

7. Verify that Docker works and has been installed correctly.

    ```bash
    sudo docker run hello-world
    ```

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

#### Building the Image

Once the Dockerfile has been completed, it is time to build the actual image. To do this, it is necessary to be in the same directory as the Dockerfile. Next, Bundler will need to be used to ensure that the Gemfile.lock has the correct version numbers for the dependencies (this is a Ruby thing that is unrelated to Docker). The "docker build" command is then used to build the image, as shown below.

```bash
cd tutorial-build
bundle install
docker build -t tutorial .
```

The "-t" option in the Docker command shown above gives the image that will be built a tag (in this case "tutorial") that can be used to refer to the image later on. Docker images are normally tracked by identifiers made up of letters and numbers, so giving an image a tag can make it easier to work with later on.

Building the image may take some time, since it will download all of the dependencies. When this is done, however, you will have a completed Docker image!

### Deploying & Running the Container
#### Using Docker Hub to Push and Pull Images
One method of sharing images is to use Docker Hub (think github for Docker images). The first step is to sign up for a Docker ID account at https://hub.docker.com/register. After registering for an account and verifying your email, sign into your Docker Hub at https://hub.docker.com. Click on the option to create a new repository. You'll have the option to set the following:

1. A namespace (i.e. does the repository belong to an organization you're a part of or just you).
2. A name for the repository. Let's use the name "tutorial" for now.
3. A short description of the repository (100 character max) and a longer description where you can put markdown.
4. A public/private option. Public repos are visible to anyone and can be accessed by anyone. Use this for projects where you want to share your work.

After creating your repository, the repo will still be blank. It's time to populate the repository with something. Go to your terminal and enter the following command:

```bash
docker images
```

This will list all images that are stored on your local machine. You should see the tutorial image we created earlier, along with some columns with additional information. We wish to associate our local image with the remote repository. In order to do this, find the image ID for our tutorial image and use the following command:

```bash
docker tag [tutorial-image-ID] [Docker-username]/tutorial:latest
```

If you run 'docker images' again, you should see that there is now a new image in a different repository called [Docker-username]/tutorial and tagged latest. Now we need to push our local image to our remote repository. Enter the following commands:

```bash
docker login
docker push [Docker-username]/tutorial:latest
```

The 'docker login' command will prompt you to login in to your Docker account. 'docker push' does exactly what you would think it does - pushes the image stored locally to your remote repository 'tutorial'. If you sign back into https://hub.docker.com you should see an image now in your repository. 

Now anyone with access to your repository can download and run your image. On any computer that doesn't have the tutorial image simply run the following command:

```bash
docker run [Docker-username]/tutorial 
```

Docker will automatically pull the image from the remote server and run the image.

#### Advanced Deployment

This section will cover some advanced deployment techniques. These are not part of the tutorial, so if you just want to follow the tutorial you can jump ahead to the next section, "Starting the Container". However, knowing basic information about these techniques is useful if you want to know what Docker is capable of. 

Docker Machine is an important tool that allows you to install Docker Engine (what is conventionally thought of as "Docker") on virtual hosts. These hosts can then be managed with a new set of 'docker-machine' commands. This allows older Mac and Windows systems to run Docker, or allows you to create and manage hosts on remote cloud systems such as AWS. This is the easy and efficient way to manage multiple Docker hosts locally, on a network, or on the cloud.

Swarm mode is the primary tool for achieving high scalability and performance using multiple systems. Swarm mode is a type of cluster management. You can define various nodes, which are really just an instance of the Docker Engine which will be participants in the swarm. Nodes you designate as manager nodes take a "service definition" and then assign tasks to nodes designated as worker nodes. Tasks are simply a docker container and the commands to run inside that container. Put together, swarm mode allows you to distribute workload across multiple hosts, with the guarantee that each host will be able to complete each task.

#### Starting the Container
To start running the container, it is necessary to be in the same directory as where the image was created. Then, the "docker run" command can be used to actually start running the container, as shown below.

```bash
cd tutorial-build
docker run -p 4000:4000 tutorial
```

In the command shown above, the "-p" option is used to map a port on the container (in this case port 4000) to a port on the machine that is running the container (in this case also port 4000). This will direct any traffic on port 4000 of the machine to port 4000 in the container.

#### Accessing the Application
To access the tutorial site that is now running, use a Web browser and navigate to "http://0.0.0.0:4000". This should pull up the tutorial Web site running on the container!

### Terminating the Application in the Container
#### Stoppping the Container
To stop all running containers, just execute the command shown below. Stopping the container will preserve its state.

```bash
docker stop $(docker ps -a -q)
```

#### Removing the Container
Removing a stopped container will discard that container's state. To do this, execute the following command.

```bash
docker rm $(docker ps -a -q)
```

## Conclusion
This tutorial has shown you how to install Docker, make basic Dockerfiles for your programs, build Docker images, and run them. You are now free to use Docker to make distributing your programs easier!

*Note: The commands necessary for installation were taken from the Docker website.*