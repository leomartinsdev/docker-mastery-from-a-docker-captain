### 46 - Container Lifetime & Persistent Data
Containers are usually immutable and ephemeral.
"Immutable infrastructure": only re-deploy containers, never change.
This is the ideal scenario, but what about databases, or unique data?
Separation of Concerns: Ideally, the container sholdn't contain your unique data mixed-in with your application binaries.
Docker gives us features tu ensure these "separation of concerns". Ideally, the container sholdn't contain your unique data mixed-in with your application binaries.

<br>

Two solutions to this problem:
- Volumes: make special location outside of container UFS (union file system) to store unique data. You can attach that data to any container. It will look just like a local file path, it wont know its coming from the host.
- Bind Mounts: link container path to host path. It will look just like a local file path, it wont know its coming from the host.
<br>

### 47 - Persistent Data: Data Volumes
- ```VOLUME``` command in Dockerfile.

Also override with ```docker run -v /path/in/container```;
Bypasses Union File System (UFS) and stores in alt location on host;
Includes it's own management commands under ```docker volume```;
Connect to none, one or multiple containers at once;
Not subject to ```commit```, ```save```, or ```export``` commands;
By default they only have a unique ID, but you can assign name. Then it's a "named volume". Thats absolutely necessary to understand what is saved so you can connect new containers to that data.
<br>

##### Named Volume
```shell
<name-for-volume>:<volume_path>
ex:
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:var/lib/mysql mysql
```
<br>

### 49 - Persistent Data: Bind Mounting
Maps a host file or directory to a container file or directory.
Basically just two locations pointing to the same file(s).
Again, skips UFS, and host files overwrite any in container.
Can't use in Dockerfile, must be at ```container run```.
- ```... run -v //c/Users/username/stuff:/path/container``` (windows)
- ```... run -v /Users/username/stuff:/path/container``` (unix)

```shell
cd section-5
cd dockerfile-sample-2
docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
docker container exec -it nginx bash
New-Item -ItemType File -Name testme.txt
docker container exec -it nginx bash // testme.txt is here and I created it on a shell outside the container, so they're sharing the same location
echo "is it me you're looking for" > testme.txt
```

localhost/textme.txt -> contains the text "is it me you're looking for".

### 52. Assignment: Database Upgrades with Named Volumes
- Create a postgres container with named volume psql-data using version 16;
- Use Docker Hub to learn VOLUME path and versions needed to run it;
- Check logs, stop container;
- Create a new postgres container with same named volume using 17;
- Check logs. It should have only a couple of lines of startup logs because it doesn't have to do all the startup work.

```shell
docker container run --name postgres16 -e POSTGRES_PASSWORD=password -v postgres-db:/var/lib/postgresql/data postgres:16

docker container logs postgres16
docker container stop postgres16

docker container ls -a

docker container run --name postgres17 -e POSTGRES_PASSWORD=password -v postgres-db:/var/lib/postgresql/data postgres:17

docker container logs postgres17
```

What I saw in the postgres17 logs: PostgreSQL Database directory appears to contain a database; Skipping initialization.

So, that means it worked, they're using the same data volume.
