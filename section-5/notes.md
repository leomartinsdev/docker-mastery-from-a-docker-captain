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
