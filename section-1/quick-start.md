 #### Docker Image
 Universal app packager. Cross-OS. Cross-platform. Application language agnostic.

#### Dockerfile
Recipe/set of instructions to make a container image. Docker Image = Container Image.
- FROM: starting point, e.g: python.
- RUN: command to run after the starting point.
- WORKDIR: changes locations in the file system.
- COPY: copy our source code from the host we are building on into its own lawyer stacked on top of the others.

#### Registry
Universal app distribution. It's the standard in the industry on how to move containers around.

<code>docker push</code> -> pushes the image to a registry.
<code>docker pull</code> -> pulls the image from a registry to a machine.