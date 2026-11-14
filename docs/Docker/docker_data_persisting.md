# Persisting Data in Containers

Normally data in a container is lost if the container is deleted.

## Benefits

- Persistant data (e.g., database files, logs)
- Share data between containers
- Seperate app code from user data
- Avoid losing data when a container stops or is deleted

## Ways to persist data

### Docker Volumes

**Volumes**: Storage areas on the Docker Host that are managed by Docker (stored under `/var/lib/docker/volumes/`)
- Each volume gets its own volume path as follows:
	- `/var/lib/docker/volumes/<VOLUME-NAME>/_data`
- Multiple containers can be mounted in one volume.
- Isolated from the container layers and its file system.
- Persists independent of the containers.
- Is mounted into the containers at runtime.
- Allows data to survive restarts, deletion and image rebuilds.
- IMPORTANT: If Docker Host has a problem, then data is in danger.

```bash
docker volume create <VOLUME-NAME>
	# Creates a new named docker volume
```

```bash
docker volume ls
	# Listing existing docker volumes
```

```bash
docker volume inspect <VOLUME-NAME>
	# Shows detailed info about a volume
```

```bash
docker volume rm <VOLUME-NAME>
	# Delete a docker volume
```

```bash
docker run -v <VOLUME-NAME>:<CONTAINER-DIRECTORY> <IMAGE>
	# Example: "docker run -v mydata:/app/data ubuntu"
	# You can also give the container read access only to the volume
		# "docker run -v mydata:/app/data:ro ubuntu"
```

```bash
docker run -it --volumes-from <CONTAINER-FROM (EXISTS)> <CONTAINER-TO (NEW)>
	# Create a container and allow it to inherit all volumes from another container
	# Example: "docker run -it --volumes-from cont1 ubuntu"
```

### Docker Bind Mounts

**Bind mounts**: Any file or directory on the Docker Host that is mounted into the container. Docker does not manage it.
- It is used with docker run commands (not in a Dockerfile).
- Used for development and testing, because in Docker volumes its highly not recommended to touch the file system (in Bind Mounts can the container directly access host files).

```bash
docker run -v <HOST-PATH>:<CONTAINER-DIRECTORY> <IMAGE>
	# "/app" in the container is mapped directly to the host path
	# Example 1: "docker run -v /home/user/project:/app my-dev-image"
	# Example 2: "docker run -v $(pwd):/app my-dev-image"
	# Example 3: "docker run -v /home/user/nginx.conf:/etc/nginx/nginx.conf nginx"
```
