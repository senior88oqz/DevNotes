# Docker Notes

## Image (`docker run`) vs Container (`docker create`)

//TODO

## Work with Storage and Volume

### Types

There are three types of storage mounted can be mounted into containers.

Mount points can be created with `--mount` flag in `docker run` and `docker create`

#### Bind mounts

* Attach parts of the host file system into a container
* Can be re-mounted to other locations
* Read only mount (`readonly=true`) to prevent write operations from the container
* Sharing with multiple containers must point to the *host specific* exact locationon
* **It’s better to
avoid these kinds of specific bindings in generalised platforms or hardware pools**

**TL;DR**

* Create with `--mount type=bind`

```bash
# location must be in absolute path
docker run --name container_name \
--mount type=bind, src=source_location, dst=dst_location, [readonly=true] \
image_name
```

#### In-memory storage

* Sensitive or temporary data is not written to image, i.e.,
  * Private key files
  * Database passwords
  * API key files
* The mount point is created with sensible defaults for generic workloads

**TL;DR**

* Create with `--mount type=tmpfs`

```bash
# Any files created under /tmp will be written to memory
docker run --name container_name \
--mount type=tmpfs,dst=/tmp \
[--entrypoint mount] \
image_name -v
```

* Outputs:

```bash
tmpfs on /tmp type tmpfs (rw,nosuid,nodev,noexec,relatime)
```

- [ ] `tmpfs` device is mounted to the tree at `/tmp`
- [ ] the tree is read/write capable
- [ ] suid bits will be ignored on all files in this tree
- [ ] no files in this tree will be interpreted as special devices
- [ ] no files in this tree will be executable
- [ ] file access times will be updated if they are older than the current modify or change time

#### Docker volumes

* Decouples storage from specialized locations on the hsot file system (as in bind mounts)
* Sharing access to data is a key feature of volumes
* Provides container-independent data management
* Images are appropriate for packaging and distributing relatively
static files like programs (reusable); volumes hold dynamic data or specialisations (simple to share). i.e.
  * Database software versus database data
  * Web application versus log data
  * Data processing application versus input and output data
  * Web server versus static content
  * Products versus support tools

**TL;DR**

* Create and inspect volumes with `docker volume create` and `docker volume inspect`

```bash
# By default Docker creates volumes using the local volume plugin.
docker volume create \
--driver local \
--label [key]=[value] \
volume_name

# To checkout a volume
docker volume inspect volume_name
```

* Create anonymous volumes

```bash
# Useful when you need to eliminate potential volume naming conflicts
docker run --name container_name \
--mount type=volume,dst=/library/TAoCP.vol1 \
--mount type=volume,dst=/library/TAoCP.vol2 \
image_name
```

**Note** : Anonymous volumes are automatically cleaned up when containers are deleted via the `docker run --rm` or `docker rm -v` flags.

* List and remove volumes

```bash
docker volume ls|list

# No volume that is currently in use can be deleted.
docker volume remove volume_name|volume_id

# Deleteall volumes that can be deleted.
# Confirmation can be supress with --fore or -f flag
docker volume prune [--filter [label_key]=[label_value]]
```

* Mount docker volume with `--mount`

```bash
# a new volume will be created if volume_name doesn't exist
docker run -d \
--mount src=[volume_name],dst=dst_location \
--name container_name \
image_name
```

**NOTE** : New users should try --mount syntax which is simpler than --volume syntax.

* Copy the mount definitions from other container(s) with `volumes-from`

```bash
# Copies the full volumes definitions (mount points, permissions, etc.)
docker run --name container_name \
--volumes-from container_1 \
--volumes-from container_2 \
image_name
```

##### Advanced storage with volume plugins

There are community and vendor plugins will help you solve the hard problems associated with writing files to disk on one machine and depending on them from another.

Rexray(<https://github.com/rexray/rexray>) is a popular open source project that provides volumes on several cloud and on-prem storage platforms.

### Share data between host and container

### Share data between containers

## Best Practices

### `ENTRYPOINT` and `CMD`

* Docker recommends using `ENTRYPOINT` to set the image’s _main command_, and then using `CMD` as the _default flags_. Here is an example Dockerfile that uses both instructions.

```docker
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

### `COPY` and `ADD`

* For other files/directories that do not require `ADD`’s tar auto-extraction capability, should always use `COPY`.
* The files least likely to be changed should be in lower layers, while the files most likely to change should be added last.

## Reference

[Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#add-or-copy)
[Docker ENTRYPOINT & CMD: Dockerfile best practices](https://medium.freecodecamp.org/docker-entrypoint-cmd-dockerfile-best-practices-abc591c30e21)