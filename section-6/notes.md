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