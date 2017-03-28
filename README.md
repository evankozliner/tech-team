# tech-team

See the [live site](https://evankozliner.github.io/tech-team/)

# To start working

```bash
gem install jekyll bundler
cd tutorial
bundle exec jekyll serve

```

# Important Note
The site needs to be distributed with the proper Gemfile.lock. There were issues with the github-pages gem, so this has been removed from the Gemfile for distribution.

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

