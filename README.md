# Docker Docs
## Basic Docker Commands and Flags
### Docker Commands
1. docker pull <image>: Download an image from a registry, like Docker Hub.
2. docker build -t <image_name> <path>: Build an image from a Dockerfile, where <path> is the directory containing the Dockerfile.
3. docker image ls: List all images available on your local machine.
4. docker run -d -p <host_port>:<container_port> --name <container_name> <image>: Run a container from an image, mapping host ports to container ports.
5. docker container ls: List all running containers.
6. docker container stop <container>: Stop a running container.
7. docker container rm <container>: Remove a stopped container.
8. docker image rm <image>: Remove an image from your local machine.

### Common Flags (only for docker run)
--name: Assign a name to the container.
-p, --publish: Publish a containers port(s) to the host. Useful if you want to enter the services inside of the Container from outside of the Container.
-e, --env: Set env variables inside the container. (Can be used multiple times for multiple variables).
-d, --detach: Run the container in detached mode (in the background + no logs in console).
-v, --volume: Bind mount a volume from the host to the container.

## Building Dockerfiles
### Structure (with every command)
|Command|Explanation|
|-|-|
|FROM|Base Image you will build on|
|WORKDIR|Sets working dir for every Command (if it does not exist it will be created)|
|COPY|Copies files and dirs from host into containers file system|
|ADD|Similar to COPY, but can also handle remote URLs and automatically unpack archives|
|RUN|Executes a command within the image as a new layer|
|ENV|Sets environment variables for the container|
|EXPOSE|Informs Docker that the container will listen on the specified network ports at runtime|
|ENTRYPOINT|Defines the default command to execute when running a container from the image (can not be overridden)|
|CMD|Defines the default command to execute when running a container from the image (can be overridden)|

### Efficient Layer Caching
Docker creates a new layer for each instruction (e.g., RUN, COPY, ADD, etc.) in the Dockerfile.If the instruction hasn’t changed since the last build, Docker will reuse the existing layer.

1. Minimize changes in the Dockerfile: Try to minimize the frequency of changes in your Dockerfile, and structure your instructions in a way that most frequently changed lines appear at the bottom.
2. Use smaller base images: Smaller base images reduce the time taken to pull the base image as well as the number of layers that need to be cached.
3. Combine multiple instructions: In some cases, combining instructions (e.g., RUN) can help minimize the number of layers, making caching more efficient.

### Making the Dockerimages small and secure
#### Image Size
1. Use an appropriate base image: hoose a smaller, more lightweight base image that includes only the necessary components for your application. For example, consider using the alpine variant of an official image, if available, as it’s typically much smaller in size.
2. Run multiple commands in a single RUN statement:  Combine multiple commands into a single RUN statement using && to minimize the number of layers and reduce the final image size.
3. Remove unnecessary files in the same layer: (look into 2 so in the same RUN command remove these files).

#### Secure Dockerimages
1. Keep base images updated.
2. Avoid running containers as root.
3. Limit the scope of COPY or ADD (using COPY . . may unintentionally include sensitive files).
4. Scan images for vunerabilities (Clair->open source and free).

## Basic Docker Data Persistence:
### Volume Mounts
Definition: Volume mounts are a way to map a folder or file on the host system to a folder or file inside a container

Step 1: Creating a volume
```sh
docker volume create my-volume
```

Step 1.5: If you want you can inspect the details of the volume
```sh
docker volume inspect my-volume
```

Step 2: Mount the Volume in a Container (with either -v or --mount)
```sh
docker run -d -v my-volume:/data your-image
```
```sh
docker run -d --mount source=my-volume,destination=/data your-image
```

Step 2.5(optional): Share the Volume between Container
```sh
docker run -d -v my-volume:/data1 image1
docker run -d -v my-volume:/data2 image2
```

Step 3: Remove the Volume:
```sh
docker volume rm my-volume
```
## Docker Compose and associated Commands
To create a docker-compose.yml file, start by specifying the version of Docker Compose you want to use, followed by the services you want to define.

### Example of a Docker Compose
```sh
version: "3.9"
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    depends_on:
      - db
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: mysecretpassword
```

### Commands inside a Docker Compose
|Command|Explanation|
|-|-|
|version|Specifies the version of the Docker Compose file format to use.|
|services|Defines the containers (services) that make up your application.|
|build|Configures build options for creating Docker images from a Dockerfile.|
|image|Specifies the Docker image to use for the service.|
|container_name|Sets a custom name for the container.|
|ports|Maps container ports to host ports to enable external access.|
|volumes|Defines mounts for data persistence and sharing between containers.|
|environment|Sets environment variables inside the container.|
|networks|Configures custom networks for inter-container communication.|
|depends_on|Specifies dependencies between services to control startup order.|
|command|Overrides the default command specified in the Docker image.|
|entrypoint|Overrides the default entrypoint of the Docker image.|
|restart|Configures restart policies for the containers.|
|healthcheck|Defines health checks to ensure the container is running properly.|

### Commands for a Docker Compose
|Command|Explanation|
|-|-|
|docker-compose up|will read the .yml file and start the defined services|
|docker-compose down|Stops and removes all running containers, networks, and volumes defined in the .yml file|
|docker-compose ps|Lists the status of all containers defined in the .yml file|
|docker-compose logs|Displays the logs of all containers defined in the .yml file|
|docker-compose build|Builds all images defined in the .yml file|