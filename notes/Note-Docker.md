---
layout: page
title: Docker
category: note
tags: Others
---

```shell
# version
docker version
docker info

# login
docker login

# search images
docker search python

# list images
docker image ls

# download image
docker pull python:latest

# run in a new container
docker run -it --name='test' python bash

# run in a running container
docker exec -it container_name bash

# commit image
docker commit -m "comment" -a "author" container_id my_hub/my_image:tag

# push image
docker login
docker push my_hub/my_image

# remove image
docker rmi my_image:tag

# save image
docker save -o my_image.tar my_hub/my_image

# load image
docker load --input my_image.tar

# list containers
docker container ls
docker container ls -a

# start container
docker start container_name/container_id
docker stop container_name/container_id

# remove container
docker rm container_name
docker rm container_id
```