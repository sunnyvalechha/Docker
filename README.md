Docker: 
* Docker is a platform where we run virtaul machines as a container with minimum system dependencies.

Containers: 
* A container is a bundle of application and application libraries required to run your application in the minimum system dependencies.
* A container is an advanced version of a virtual machine, and Docker is a tool that creates containers as virtual machines.
* Containers are light in weight (size), and they are isolated from the underlying infrastructure.
* Virtualization is a technology that is used to create virtual representations of servers, storage, and networks on physical machines.

Docker Architecture's Core components:
1. Docker Daemon (background process)
2. Docker Engine
3. Docker Client (CLI where run docker commands)
4. Docker registry (Docker Hub UI) 

<img width="659" height="591" alt="image" src="https://github.com/user-attachments/assets/5ff85956-cf76-4e9c-a24c-18d367b1cb1f" />

Docker daemon: 
* The Docker daemon (known as dockerd) is the actual process that runs the containers.
* It is a background process that runs on the host operating system and manages the lifecycle of Docker containers.
* Docker daemon has a REST API (called the Docker Engine API), which is a way for other programs to talk to docker daemon.
* Docker client is an example of such a program. Whenever we use a "docker" command, the client uses the API to tell Docker daemon what it should do.

Docker Client:
* It is a CLI utility that allows users to interact with the Docker daemon.
* Docker client communicates with the Docker daemon via the REST API.
* The client uses the REST API to send requests to the daemon to do things like build, run, and manage containers.
* Docker client can run on the same host as the Docker daemon, or it can run on a separate machine and communicate with the Docker daemon over the network.

Docker registry:
* Docker registry is a central storage location for uploading and downloading Docker images.
* The most common registry is Docker Hub, where developers can publish their images for others to use.
* Registries can be public or private.
* Anyone who have access to the registry/image can download the image and run as a container.

Note: Below commands are run through user root and a regular does not have a permission to run docker commands, this is a drawback which docker have. So add your localuser to group docker and Logout and Login back from localuser

        docker run hello-world      # Create 1st Image
        docker images               # Check created images
        sudo usermod -aG docker ec2-user
        grep docker /etc/group

Dockerfile:

        # Image used is ubuntu
        FROM ubuntu:latest
        # Set the working directory where we want to place all content
        WORKDIR /app
        # Copy the content from local to docker image in specific folder
        COPY . /app
        # Install necessary packages
        RUN apt-get update && apt-get install python3 python3-pip -y
        # Set env variable
        ENV NAME World
        # This command should run to start the container
        CMD ["python3","app.py"]


**Points to remember**: Difference between "Entry Point" and "CMD"? 

* When anyone runs the "Docker run" command, both "Entry Point" and "CMD" serve as your starting command.
* "Entry Point" is something we cannot change. We cannot override this value in the docker image.
* "CMD" is configurable in the docker image. Example: the port & the IP address is configurable in CMD.
* The options mentioned in the Entry point can be mentioned in the CMD but when we don't want the someone to change the executable like I want to run with python3 only, then we will use as a "EntryPoint"        



**Run the docker file**

        docker build -t ubuntu:latest .

**Key points:**

* The -t flag assign names to the image and it is optional because in the system 100's of image might present.
* The . at the end specifies current directory
* Your Dockerfile should be named "Dockerfile" (case-sensitive)
* If your Dockerfile has a different name, you would use the -f flag

**Example:** docker build -t docker-image:latest -f MyDockerfile .

**Purpose** of creating a image is run the application.

**Steps:**

1. Create Dockerfile
2. Docker image created
3. Run the image to create a container

        docker run -it sunny/ubuntu:latest /bin/bash

**Key points:**

* -it = interactive terminal (keeps STDIN open and allocates a pseudo-TTY)
* /bin/bash = starts a Bash shell inside the container

          docker ps                                # list running containers
          docker ps -a                             # list running / non-running containers
          docker login                             # login to dockerhub
          docker push sunny/ubuntu:latest          # push image to docker hub
          docker pull <image-name>                 # pull any image from docker hub




# Multi-stage Docker builds & Distro-less images

**Multi-stage builds** is a feature that allows you to use multiple FROM statements in a single Dockerfile. Each FROM statement marks the beginning of a new stage, and each stage can have a different base image.

**Benefits**
* Smaller image size: By excluding unnecessary build dependencies and development tools from the final image, multi-stage builds significantly reduce the image size.
* Enhanced security: Smaller images with fewer components reduce the attack surface and potential vulnerabilities in production deployments.
* Improved build performance: Docker can cache intermediate layers in multi-stage builds, speeding up subsequent builds when only changes are made to later stages.
* Simplified Dockerfile maintenance: Multi-stage builds help organize your Dockerfile, making it more modular and easier to maintain by separating concerns into distinct stages.

**Distroless images** are minimal Docker images designed to contain only your application and its essential runtime dependencies, stripping out the rest of a typical Linux distribution.

Q: A production issue that faced and a solution?
A: Earlier Ubuntu, RedHat, or Python runtime base images were used, which were exposed to some kind of vulnerabilities by hackers. We moved to a Python distro-less image that only has the Python runtime and does not have any other basic packages like 'ls' or 'cd'. So this provides us with a high level of security.
That's how our applications are not exposed to any OS or Os related vulnerabilities.

Q: How to find distroless images?
A: Google > distroless images > Github > https://github.com/GoogleContainerTools/distroless
Go to a folder like Python> readme.md > base image


**Practical:**

        yum install go -y
        yum install git
        git --version
        mkdir abhiveera
        cd abhiveera/
        git clone https://github.com/iam-veeramalla/Docker-Zero-to-Hero.git
        cd /home/ec2-user/abhiveera/Docker-Zero-to-Hero/examples/golang-multi-stage-docker-build/dockerfile-without-multistage
        go run calculator.go

Now, containerize the application

        docker build -t simplecalci .
        docker images

Note: The Size of the image should be around 800-900 MB

Go back 1 folder at path - /home/ec2-user/abhiveera/Docker-Zero-to-Hero/examples/golang-multi-stage-docker-build/

<img width="459" height="456" alt="image" src="https://github.com/user-attachments/assets/c2cae46d-98de-4c89-87c4-a3bf1046b71e" />

* Here, the docker file is split into 2 stages.
* Stage 1 we don't have a Entry-point of CMD
* Stage 1 is only dedicated to build steps
* **Stratch** - Minimalistic distroless image

By moving towards multi-stage builds and distroless images we not only reduce the size of the images but also make sure that the containers we have are running securily and these images are very less vulnerable to the threats.

# Bind mounts & Volumes

**Bind mount**: 
* A file or directory on the host machine is mounted with a container.
* Bind mount creates direct link between a host system path and a container allowing access to files or directories stored anywhere on the host.

**Volume**: 
* Whenever we create a volume using "docker volume create" command and docker itself create a directory.
* Volumes retain the data even after the containers using the volumes are deleted.
* Volume data is stored on the host OS but to access the volume data we must mount the volume directory with the container.

When to use:
* When you want to create or generate files in a container and persist the files onto the host's filesystem.
* Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting **/etc/resolv.conf** from the host machine into each container.
* Docker volume can be shared between host to container and container to container.


Why: Containers are lightweight and utilize the host operating system's resources (CPU, memory)?.
* Suppose there is an application running on a container and a application also have a log file and suddenly the container goes down.

**Practical:**

* docker -v | docker --mount                # both the commands works same
* docker volume ls                          # list all volumes
* docker volume create dev-volume           # create new volume

Note: Above, we have created a volume, now we can dedicate this partition / volume to one container or multiple containers.

* docker volume inspect dev-volume          # get information about the volume

Note: To delete the volume first stop the containert then delete the container.

* docker volume rm sunnyvol                 # delete a volume
* docker run -d --mount source=sunny,target=/app nginx:latest
* docker inspect a056b606c3fa | grep Mount

<img width="842" height="233" alt="image" src="https://github.com/user-attachments/assets/4c6cd8dc-f8e2-47cd-888e-7dece6fac70e" />


The directory "/var/lib/docker/volumes/sunny/_data" will be created at the local machine.












 
  
