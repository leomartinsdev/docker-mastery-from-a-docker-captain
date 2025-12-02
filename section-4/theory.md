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

docker container ls     -> lists running containers
docker ps               -> lists runing containers
docker container ps -a  -> lists all containers (running and stopped)
docker container ls -a  -> lists all containers (running and stopped)

docker container stop <container_id> -> stops the container

docker container run -d --publish 80:80 --name webhost nginx -> specifices the name

docker container logs <container_name> -> logs for the specified container.

docker container rm <container_id_1> <container_id_n> -> deletes the specified containers
docker container rm -f <container_id> -> forces the deletion of the container
```


#### Assignment: Manage Multiple Containers
```shell
docker container run -d --publish 8080:80 httpd
docker container run -d --publish 80:80 nginx
docker container run -d --publish 3306:3306 --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
docker container ls
docker container logs 28fec
docker stop 28fec b567 a931
docker rm 28fec b567 a931
docker container ls -a

generated password for mysql = dQkCAxRjZVI52CK3iOhOYrnig3+56pU9
```

#### 24 - Whats going on in Containers: CLI Process Monitoring
```shell
docker container top        -> process list in one container
docker container inspect    -> details of one container config
docker container stats      -> performance stats for all containers
```