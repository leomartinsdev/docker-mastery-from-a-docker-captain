#### Image
An image is the application we want to run. A container is an instance of that image running as a process.
You can have many containers running off the same image.
Docker's default image "registry" is calle Docker Hub.

```shell
docker container run --publish 80:80 nginx
```

1. Downloaded image 'nginx' from Docker Hub
2. Started a new container from that image
3. Opened port 80 on the host IP
4. Routes that traffic to the container IP, port 80

```shell
-d - means "detach", so the container runs on the background.

docker container ls -> list running containers
docker container ls -a -> run all containers (running and stopped)

docker container stop <container_id> -> stops the container

docker container run -d --publish 80:80 --name webhost nginx -> specifices the name

docker container logs <container_name> -> logs for the specified container.

docker container rm <container_id_1> <container_id_n> -> deletes the specified containers
docker container rm -f <container_id> -> forces the deletion of the container
```