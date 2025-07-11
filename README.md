# Docker

Reference repo: https://github.com/iam-veeramalla/Docker-Zero-to-Hero

* docker run hello-world      # Create 1st Image

* docker images               # Check created images

Note: Above commands are run through user root and a regular does not have a permission to run docker commands, this is a drawback which docker have. So add your localuser to group docker.

* [ec2-user@ip-172-31-7-199 ~]$ sudo usermod -aG docker ec2-user

* grep docker /etc/group

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





