## Overview

This repo is there for some docs on how to use docker while reading and researching online.

The first thing to do is to install [Docker Desktop](https://www.docker.com/products/docker-desktop)

### General Commands

You might want to push your details to your docker hub repo therefor you need first to login. After the command write your credentials, such as username and password:
`docker login`

If for some reason you ever want to logout of your account use:
`docker logout`

In order to create an image, that will be used later to create a container - from a project run the following command:
`docker image build -t username/reponame:tag-name .`

To see the list of all images write:
`docker image ls`

Then to create an container from the new image run `docker container run -d --name container-name -p 8000:8080 \username/reponame:tag-name`, instead of `-d` use `-it` and at the end spec the bash with `alpine sh` if you need to interact with the terminal. If you have already runned your container once there is no need to spec the `-p port:application-port path`, instead just run `docker container run -it --name container-name apline sh`. If we use `CTR + P + Q` we get out of the terminal and keep the container running, however if we write just `exit` in the terminal it will stop the app, kill the container and get us back to our main terminal. 

To see your up and running containers write `docker container ls` and use the `-a` tag to list containers that are stoped as well.

In order for you to publich your image as a repository and be able to access it from any other PC or to have a backup run the following command:
`docker image push username/reponame:tag-name`

At some point you might want to remove your image / repository, therefore write:
`docker image rm username/reponame:tag-name`

To stop a container write. which will give the container around 10 seconds to piecefully shut down:
`docker container stop container-name`

If for some reason the container dosent stop, it will launch automatically in the background the kill command. That one can also be exectured by you directly if you want to do something quick:
`docker container kill container-name`

You can run it by using:
`docker container start container-name`
