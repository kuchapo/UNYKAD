# Docker Compose

Docker Compose can simultaneously create, start, stop or delete multiple containers on the same Docker Host.

    - Allows you to define the entire environment (services, networks, volumes, dependencies, etc.) in a single YAML file (docker-compose.yml).
    - Using a single docker command, you can execute the YAML file to run a project of multiple containers (on a single Docker Host).
    - Docker Compose can build the images using the Docker Files or pull them from a registry.

## File Structure

### services

- Defines all the different containers we need to create.
- The names of the services (containers) have significance to Docker Compose configuration only.

```docker-compose
services:
  <CONTAINER-NAME1>:
  <CONTAINER-NAME2>:
  <CONTAINER-NAME3>:
```

#### image

- For the database container, we need to create it from a mysql image (from Docker Hub).
	- `image: mysql`

```docker-compose
services:
  database:
    image: mysql
  frontend:
  backend:
```

#### build

- For the frontend container, we need Docker Compose to build the image from a file named `Dockerfile` available in the same directory as the docker-compose.yml file.
	- `build: .`

```docker-compose
services:
  database:
    image: mysql
  frontend:
    build: .
  backend:
```

- For the backend container, we need Docker Compose to build the image from a file named `fileconfig` available in the /app directory.
	- `build:`
		- `context: /usr/project/app`: Defines the full path of the Dockerfile
		- `dockerfile: fileconfig`: Defines the file's name to use

```docker-compose
services:
  database:
    image: mysql
  frontend:
    build: .
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig
```

#### container_name

- We can use the container_name to define a name for the container.

```docker-compose
services:
  database:
    image: mysql
    container_name: cont1
  frontend:
    build: .
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig
```

#### ports

- Defines the host-port:container-port mapping.

```docker-compose
services:
  database:
    image: mysql
    container_name: cont1
  frontend:
    build: .
    ports:
      - "3000:5000"
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig
```

#### volumes (bind mount)

- The `volumes:` under a service is used to create a bind mount (HostPath) mapping between a hostPath and a containerPath.

```docker-compose
services:
  database:
    image: mysql
    container_name: cont1
    volumes:
      - hostPath:containerPath
  frontend:
    build: .
    ports:
      - "3000:5000"
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig
```

#### volumes (named Volume)

- The `volumes:` can also be used to mount a named volume (volumeName) to a containerPath.
	- Requires volume to be created seperately as shown in the `volumes:` section at the bottom

```docker-compose
services:
  database:
    image: mysql
    container_name: cont1
    volumes:
      - volumeName:containerPath
  frontend:
    build: .
    ports:
      - "3000:5000"
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig

volumes:
  - volumeName:
```

#### volumes (anonymous Volume)

- Mounts an anonymous volume to the container.
	- In this case we don't need the `volumes:` section outside the `services:`

```docker-compose
services:
  database:
    image: mysql
    container_name: cont1
    volumes:
      - containerPath
  frontend:
    build: .
    ports:
      - "3000:5000"
  backend:
    build:
      context: /usr/project/app
      dockerfile: fileconfig
```

## Docker Compose Commands

Requirement: Docker Compose File should be in the same directory unless you specify it in the command.

```bash
docker compose build
	# Builds the images for the services

docker compose run <SERVICE-NAME (Container)>
	# Creates a specific Container included in the docker-compose.yml file

docker compose up
	# Builds the images if they are not located locally and starts the containers
	# So even when "docker compose build" wasn't done, it builds the images

docker compose up -d
	# Runs the containers in detached mode

docker compose -f <FILE-NAME> up
	# Specifies a different compose file (with path),
	# builds the images, and starts the containers

docker compose down
	# Stops and deletes all containers created by "docker compose up"

docker compose down --volumes
	# Stops and deletes all containers created by "docker compose up"
	# Also deletes the volumes (named and anonymous)

docker compose down --remove-orphans
	# Stops and deletes all current Compose containers and any old (orphan) containers that are no longer referenced by Compose yaml file

docker compose down --volumes --remove-orphans
	# Combines the both commands above

docker compose stop
	# Stops the containers

docker compose start
	# Starts the containers

docker compose ps
	# Shows running Docker Compose containers

docker compose ps -a
	# Shows all Docker Compose containers (also not running ones)
```

## Docker Compose Network

By default, Docker Compose sets up a bridge network for your app

    - Each container for a service joins the default network and is reachable by other compose services (containers) on that network

```bash
docker compose up -d
docker network ls
	# Will show the new bridge network
	# Docker Network will take the name of the current folder
```