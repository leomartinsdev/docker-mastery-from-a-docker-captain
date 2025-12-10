### Image
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

docker container start <container_name>    -> starts an existing container

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


### 23 - Assignment: Manage Multiple Containers
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

### 24 - Whats going on in Containers: CLI Process Monitoring
```shell
docker container top        -> process list in one container
docker container inspect    -> details of one container config
docker container stats      -> performance stats for all containers
```

### 26 - Getting a Shell Inside Containers: No Need for SSH
```shell
docker container run -it    -> start new container interactively
ex: docker container run -it --name proxy nginx bash

docker container exec -it <container_name> <program_to_run>  -> run additional command in existing and running container
ex: docker container exec -it mysql bash

exit -> exists the shell and stops the container
```

### 27 - Docker Networks: Concepts for Private and Public Comms in Containers
The ```-p``` exposes the port on your machine.
For local dev/testing, networks usually "just work".
    - "Batteries Included, but removable" -> defauls work well in many cases, but easy to swap out parts to customize it.
Each container connected to a private virtual network "bridge".
Each virtual network routes through NAT firewall on host IP.
All containers on a virtual network can talk to eachother without -p.
Best practice is to create a new virtual network for each app:
    - network "my_web_app" for mysql and php/apache containers
    - network "my_api" for mongo and nodejs containers

You can have something like this:
    - docker container port <container> 80:80
    - docker container port <container> 8080:80
    - That is because the left side is the host and the right one is the container. You can't have two containers listening on the same port at the host level.

```shell
docker container port <container> 80:80 ->  exposes the host port 80 and forwars traffic from that port into the port 80 of tha container.
docker container port <container_name>   -> shows which ports are forwarding traffic to that container from the host into the container itself.
docker container inspect --format "{{ .NetworkSettings.IPAddress }}" <container_name> -> retrieves the IP of the container.
```
<br>

### 29 - Docker Networks: CLI Management of Virtual Networks
```--network host``` - Gains performance by skipping virtual networks but sacrifices security of container model
```--network none``` - Removes eth0 and only leaves you with localhost interface in container

```shell
docker network ls   -> show networks
docker network inspect <network_name> -> inspect a network
docker network create --driver <network_name>  -> creates a network. If you dont specify driver, it creates the new VN with the bridge driver, which is the default.
docker network connect          -> attach a network to container
docker network disconnect       -> detach a network from container
```
<br>

##### Command example to create a container with an already existing network
```shell
docker container run -d --name new_nginx --network my_app_net nginx
docker network inspect my_app_net (to inspect)
```

##### Connect a container to a network
```shell
docker network connect <network_id> <container_id>
```

##### Disconnect a container to a network
```shell
docker network connect <network_id> <container_id>
```
<br>

### 30 - Docker Networks: DNS and how containers find each other
DNS is the key to easy inter-container comms. You can't rely on IP addresses because its very dynamic.
Docker DNS: docker daemon has a build-in DNS server that containers use by default.
DNS Default Names: Docker defaults the hostname to the container's name, but you can also get aliases.
Two contianers on a same network can talk to eachother using their names
Example: ```docker container exec -it <container_1> ping <container_2>```

<br>

### 31 - Assignment: Using Container for CLI Testing
```shell
docker container run -it --name centos centos:7 bash
docker container run -it --name ubuntu14 ubuntu:14.04 bash
curl --version (ubuntu doesnt have curl)
apt-get update && app-get install curl ( on ubuntu)
docker container rm centos ubuntu14
```

Correção: the --rm when creating an container makes it be deleted after you exit it.

### 34 - Assignment: DNS Round Robin Test
Create a new Virtual Network (default bridge driver);
Create two containers from elasticsearch image;
    - docker run -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "xpack.security.enabled=false" --network <your_network> -d --network-alias search elasticsearch:8.4.3
Resarch and use -network-alias search when creating them to give them an additional DNS name to respond to;
Run alpine nslook up search with --net to see the two containers list for the same DNS name;
Run rockylinux/rockylinux:10 curl-s search:9200 with --net multiple times until you see both "name" fields show.

```shell
docker network create round-robin-test

docker run -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "xpack.security.enabled=false" --network round-robin-test -d --network-alias search elasticsearch:8.4.3

docker run -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "xpack.security.enabled=false" --network round-robin-test -d --network-alias search elasticsearch:8.4.3

docker run --rm -it --network round-robin-test alpine nslookup search

docker run --rm --network round-robin-test rockylinux/rockylinux:10 curl -s search:9200
```