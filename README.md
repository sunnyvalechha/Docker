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
  
