# Docker

Reference repo: https://github.com/iam-veeramalla/Docker-Zero-to-Hero

* docker run hello-world      # Create 1st Image

* docker images               # Check created images

Note: Above commands are run through user root and a regular does not have a permission to run docker commands, this is a drawback which docker have. So add your localuser to group docker.

* [ec2-user@ip-172-31-7-199 ~]$ sudo usermod -aG docker ec2-user

* grep docker /etc/group

**Logout and Login back from localuser (ec2-user)**

**First Docker file**

    FROM ubuntu:latest
    WORKDIR /app
    COPY . /app
    RUN apt-get update && apt-get install python3 python3-pip -y
    ENV NAME World
    CMD ["python3","app.py"]




