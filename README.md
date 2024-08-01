# Docker Docs
## Basic Docker Commands:

1. docker pull <image>: Download an image from a registry, like Docker Hub.
2. docker build -t <image_name> <path>: Build an image from a Dockerfile, where <path> is the directory containing the Dockerfile.
3. docker image ls: List all images available on your local machine.
4. docker run -d -p <host_port>:<container_port> --name <container_name> <image>: Run a container from an image, mapping host ports to container ports.
5. docker container ls: List all running containers.
6. docker container stop <container>: Stop a running container.
7. docker container rm <container>: Remove a stopped container.
8. docker image rm <image>: Remove an image from your local machine.

## Basic Docker Data Persistence:
### Volume Mounts

Definition: Volume mounts are a way to map a folder or file on the host system to a folder or file inside a container

Step 1: Creating a volume
```sh
