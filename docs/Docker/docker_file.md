# Docker File

A Docker File is a text document that contains all commands a user could use manuelly on the CLI to build an application. It builds the image automatically. One Docker File means one image!

Example DOCKERFILE:

```dockerfile
FROM ubuntu
MAINTAINER John Doe
RUN apt-get update
CMD ["echo", "Hello World!"]
```

The command `docker build` reads the instructions in a docker file and creates a docker image.

## Docker Image Layers

During container runtime, image layers are read-only. To modify the image layers, you have to rebuild the image using the Dockerfile.

However, changes made inside a running container (e.g. installing `vim` via `apt install vim`) are stored in the container's writable layer. These changes do not modify the original image and are lost when the container is removed.

- `FROM` specifies the base image, which already contains layers.
- `RUN`, `COPY` and `ADD` typically create new layers.
- Each layer has its own SHA256 digest.
- Not every instruction creates a layer (e.g. `CMD`, `ENV`).

### Best Practice Tips

- Group COPY and RUN wisely to improve layer caching
- Put less frequently changed lines early (like installing dependencies)
- Avoid unnecessary RUN commands to reduce layer count and image size

| Goal               | What's Better?              | Why?                             |
| ------------------ | --------------------------- | -------------------------------- |
| Fast builds        | More layers (smart caching) | Docker can skip unchanged layers |
| Smaller images     | Fewer layers, clean builds  | Fewer intermediate files/layers  |
| Maintainability    | Readable layers             | Easier debugging and edits       |
| Layer reuseability | Split steps logically       | Base images can be reused/cached |

## Docker File Structure

### RUN

- Runs one or more commands.
- Is executed during the image build.
- Used for updating & installing.

```dockerfile
# NOT RECOMMENDED
# USES SEPERATE RUN INSTRUCTIONS IN ONE FILE
# ADDS MULTIPLE LAYERS
# BUT SOMETIMES REQUIRED
RUN apt-get update
RUN apt-get install -y vim
```

```dockerfile
# RECOMMENDED
RUN apt-get update && apt-get install -y vim git
```

### CMD

- Is executed after the image build.
- Will be executed after the RUN instruction.
- Only the last CMD instruction will take effect.

```dockerfile
CMD ["<EXECUTABLE>", "param1", "param2"]

# Example 1
CMD ["echo", "Hello World"]

# Example 2
CMD ["echo", "Hello World,", "I'm ready to learn."]
```

### EXPOSE

- Documents which port the container listens on.
- Is only informational and does not publish the port to the host.
- It is useful for tools that auto generate configurations (Docker Compose, Swarm, K8s)

```dockerfile
EXPOSE <PORT>

# Example 1
EXPOSE 80

# Exampe 2
EXPOSE 80 443
```

### COPY

- Copies files or directories from `<SRC>` and adds them to the filesystem of the image at the path `<DEST>`.

```dockerfile
COPY <SRC> <DEST>
	# Copies files ONLY from the Build Context !
	# If <DEST> isn't available, it will create the path

# .
# ├── Dockerfile
# └── app/
#    ├── main.py
#    └── utils/
#        └── helper.py

# Example 1
COPY app/utils /app/code/utils

# Example 2
COPY app/utils /app/code/utils
COPY app/main.py /app/code/

# Example 3
COPY package*.json /app/
RUN npm install
COPY src/ /app/src/
```

!!! warning "Caution: Order matters for the build cache"
    Docker caches most instructions as a layer. If the input of an instruction changes (e.g. a copied file), that layer and all following layers are rebuilt. -> Put what rarely changes first, what often changes last.
    In Example 3, `RUN npm install` only depends on `package*.json`, so only that file is copied before it. Changes to `src/` then don't trigger a new `npm install`.

### WORKDIR

- Defines the current working directory of the docker container.
- `<PATH>` will be resulting in the image during build time and in the container during runtime

```dockerfile
WORKDIR <PATH>
	# Is equivalent to "cd <PATH>" in the container
	# If "PATH" is not available, then it becomes equivalent to "mkdir <PATH>"

# .
# ├── Dockerfile
# └── app/
#    ├── main.py
#    └── utils/
#        └── helper.py

FROM python:3.10-slim

# Example 1
WORKDIR /usr
COPY app/utils /usr/src/app/code/utils
COPY app/main.py /usr/src/app/code
CMD ["python3", "src/app/code/main.py"]

# Example 2
WORKDIR /usr/src/app/code
COPY app/utils utils
COPY app/main.py .
CMD ["python3", "main.py"]
```

!!! info
    The last `WORKDIR` instruction determines the working directory when the container starts.
    For example, commands executed using `docker exec` will use this directory as their working directory by default.

### ENV

- Allows to set environment variables for the Docker container

```dockerfile
ENV <KEY>=<VALUE>

FROM alpine

# Example 1
ENV name=EXAMPLE-NAME
CMD ["echo", "$name"]

# Example 2
# Installs ping and curl, sets variables and uses shell as execution
# "-c" lets the string interpret as a command
# "exec" is not a docker command in this case!
RUN apk add --no-cache iputils curl
ENV action=ping
ENV target=8.8.8.8
CMD ["sh", "-c", "exec $action $target"]
```

!!! info
    After building Example 2, you can override the environment variables when starting the container. For example: `docker run -e action=curl -e target=9.9.9.9 mybuiltimage`. The `-e` flag allows you to set or override individual environment variables.

### LABEL

- Used to add metadata to an image.
- Metadata can include information about the image, such as author, version, description, environment or custom data.

```dockerfile
LABEL <KEY>="<VALUE>"

# Example 1
LABEL Owner="Example Academy Team team@support.example.com"
```

## Docker File Examples

```dockerfile
FROM ubuntu
WORKDIR /app
RUN echo "This is the /app directory" > app.txt
WORKDIR /app/code
RUN echo "This is the /code directory" > app-code.txt
CMD ["ls"]

# On Docker Host: "docker build -t imageworkdir ."
# On Docker Host: "docker run -it imageworkdir bash"
# In container you can check now with "ls" and "cat"
```

```dockerfile
FROM ubuntu
WORKDIR /app
RUN echo "This is the /app directory" > app.txt
WORKDIR /app/code
RUN echo "This is the /code directory" > app-code.txt
WORKDIR /app/database
CMD ["ls"]

# On Docker Host: "docker build -t imageworkdir2 ."
# On Docker Host: "docker run -it imageworkdir2 bash"
# In container you can check now with "ls" and "cat"
```

```dockerfile
FROM ubuntu
RUN apt-get update && apt-get install -y iputils-ping
ENV action=ping target=8.8.8.8
LABEL maintainer="John Doe" team="Academy Team" version="1.0"
CMD ["sh", "-c", "$action $target"]

# On Docker Host: "docker build -t correct ."
# On Docker Host: "docker run correct"
```
