## Section 5 - Container Images, where to find them and how to build them

### 36 - What's in an image (and what isn't)
- It's the application binaries and dependencies
- Metadata about the image data and how to run the image
- Official definition: "an image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container run time."
- Not a complete OS. No kernel, no kernel modules (drivers).
- Small as one file (your app binary) like a golang static binary.
- Big as Ubuntu distro with apt, and apache, php and more installed.

<br>

### 38 - Images and their layers: discover the image cache
```shell
docker history <image_name>         -> history of image layers
docker image inspect <image_name>   -> returns a json metadata about the image
```

We don't need to download layers we already have, and as we make changes to our images, they create more layers, and if we decide we want to have the same image as the base image for more layers, it only ever stores one copy of each layer. That saves up lots of space.

<br>

### 39 - Image Tagging and Pushing to Docker Hub
The tag is not quite a version/branch. It's a pointer to a specific image commit.

##### Retags an image. This changes the repository (name) of the image. You can specify tags as <image_repository>:<image_tag>
```shell
docker image tag <image_repository> <new_image_repository>
```

##### Log in into Docker Hub
```shell
docker login
```

##### Log out of Docker Hub
```shell
docker logout
```

##### Push an image to Docker Hub
```shell
docker push <repository_name>
```

### 40 - Building Images: The Dockerfile Basics

##### Build a Docker from Dockerfile
```shell
docker build -f some-dockerfile
```
The ```-f``` specifies Dockerfile name/location

Main parameters on a Dockerfile:
- FROM - base image for the new image being built. Sets the foundation and initial environment upon all other instructions will run.

- ENV - sets envinronment variables.

- RUN - build commands to be executed. They're run top down. All the commands chained by a "&&" stays on the same layer, which saves time and space.
The proper way to log into a container is not to log into a logfile. Docker actually handles all of our logging for us. We just have to make sure everything we want to be logged is spit out to /stdout and /sterr.

- EXPOSE - describe which ports your application is listening on.

- CMD - specify default commands. Its a required parameter and its the final command that will be ran everytime you launch a new container from the image or everytime you restart a stopped container. Ex: "npm run start".

### 41 - Building Images: Running Docker Builds
```shell
docker image build -t customnginx .
```

This is the modern, structured Docker command.
Works exactly like the ```docker build``` command of the previous class.

The ```t``` tags the image as customname:latest (Default tag)
The ```.``` basically says to build the dockerfile in this directory.
