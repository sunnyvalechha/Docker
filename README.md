# Docker

Reference repo: https://github.com/iam-veeramalla/Docker-Zero-to-Hero

Docker: Docker is a platform where we run virtaul machines as a container with minimum system dependencies.

Containers: A container is a bundle of application and application libraries required to run your application in the minimum system dependencies.

Docker daemon: The Docker daemon is the heart of the Docker system. It's responsible for managing the entire Docker lifecycle. 

        docker run hello-world      # Create 1st Image

        docker images               # Check created images

Note: Above commands are run through user root and a regular does not have a permission to run docker commands, this is a drawback which docker have. So add your localuser to group docker.

          sudo usermod -aG docker ec2-user

          grep docker /etc/group

**Logout and Login back from localuser (ec2-user)**

**First Docker file**

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

# Docker bind mounts & Volumes

Why: Containers are lightweight and utilize the host operating system's resources (CPU, memory).
* Suppose there is an application running on a container and a application also have a log file and suddenly the container goes down.
* 




 
  
