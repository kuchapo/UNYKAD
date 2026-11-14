# Docker Registry

A Docker registry is a storage location or service where Docker images are stored, managed, and distributed.

- Most common one ist Docker Hub.
- There are public and private repos.

## Docker Image Naming Convention

- Docker Hub has a specific naming convention for Docker images that helps with organization, versioning and discovery.

```text
[registry/][username_or_org/]repository[:tag]

# Registry (Optional): Domain of the container registry - default to "docker.io"
# Username or organization: Name of the user or organization (required for non-official images only)
# Repository: Name of the image or the project
# Tag (Optional): Version - defaults to latest if not included
```

### Examples

```docker
docker tag myapp registry.my.corp.com/devops/myapp:1.0.0
	# Image is ready to be pushed to the private registry
	# If it should be pushed on Docker Hub -> leave the registry part empty
```

## Docker Push

How it works:

```bash
# Create Account on Docker Hub
# Create a Repository in Docker Hub (e.g. "app")
# Choose the Docker image you want to push (e.g. "app")

docker login -u labuser701

docker tag app labuser701/app:v1
nano Dockerfile
# Make changes
docker build -t app:v2 .
docker tag app:v2 labuser701/app:v2

docker push labuser701/app:v1
docker push labuser701/app:v2
```