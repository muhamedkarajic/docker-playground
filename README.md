## Overview

This repo is there for some docs on how to use docker while reading and researching online.

The first thing to do is to install [Docker Desktop](https://www.docker.com/products/docker-desktop)

### General Commands

You might want to push your details to your docker hub repo therefor you need first to login. After the command write your credentials, such as username and password:
`docker login`

In order to create an image, that will be used later to create a container - from a project run the following command:
`docker image build -t username/reponame:tag-name .`

To see the list of all images write:
`docker image ls`

Then to create an container from the new image run: 
`docker container run -d --name container-name -p 8000:8080 \username/reponame:tag-name`

To see you up and running containers write:
`docker container ls`

In order for you to publich your image as a repository and be able to access it from any other PC or to have a backup run the following command:
`docker image push username/reponame:tag-name`

At some point you might want to remove your image / repository, therefore write:
`docker image rm username/reponame:tag-name`

To stop a container write:
`docker container kill container-name`

You can run it by using:
`docker container start container-name`