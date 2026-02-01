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
```VOLUME``` command in Dockerfile.
Also override with ```docker run -v /path/in/container```
Bypasses Union File System (UFS) and stores in alt location on host.
Includes it's own management commands under ```docker volume```.
Connect to none, one or multiple containers at once.
Not subject to ```commit```, ```save```, or ```export``` commands.
By default they only have a unique ID, but you can assign name. Then it's a "named volume". Thats absolutely necessary to understand what is saved so you can connect new containers to that data.
<br>

##### Named Volume
```shell
<name-for-volume>:<volume_path>
ex:
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:var/lib/mysql mysql
```
<br>
