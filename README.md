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

At some point you might want to remove your image / repository, therefore write `docker image rm username/reponame:tag-name`, if we want to delete a runnning container we use the `-f` flag.

To stop a container write. which will give the container around 10 seconds to piecefully shut down:
`docker container stop container-name`

If for some reason the container dosent stop, it will launch automatically in the background the kill command. That one can also be exectured by you directly if you want to do something quick:
`docker container kill container-name`

You can run it by using:
`docker container start container-name`

## Docker Compose and Swarm Mode

Docker compose (docker-compose.yml) is a file where we can have the needed spec for each service (e.g. target, piblished port, where the build files are located). We can have multiple services (multiple apps) in the same container with it, with the command `docker-compose version` you can check the version of it.

In order to run the services use `docker-compose up -d`, which will start the scripts in the `docker-compose.yml` file.

Dockers swarm mode lets us put multiple docker hosts into one cluster. It has manager and worker nodes. Managers need to be highliy available, and it's recomended that we have an odd number of them - because if a network issue happens it could be that both sides have equal managers and therefor equal priority. We require swarm mode for unlocking additional features such as services.

In order to start with `ipconfig` and initialise the swarm with `swarm init --advertise-addr=172.20.192.1`. Then in order to get a new manager to the swarm we run `docker swarm join-token manager`. Use then the token command and past it in the other enviroments, to make those managers of the swarm. One of them will be the leader and others will be followers. Followers can however, act as leaders if the leader goes down. To make a woker simply run `docker swarm josin-token worker` and put the commands with the token to the nodes you can to make workers.

The next comand to create a service is `docker service create --name container-name -p display-port:instance-port \--replicas 3 username/repo-name:tag-name`

To list all services `docker service ls` which will list one contianer, however if we do `docker container ls` we get 3 containers since we have 3 replicas of the same service. This will be only acceptable to the one node in the swarm, if we have multiple nodes, a better command would be `docker service ps container-name`.

To scale up or down we just do `docker service scale container-name=10`, where 10 represents 10 containers or replicas of the same service.
 
At some point we will want to deploy the image, we can do that with the following command: `docker stack deploy -c docker-compose.yml the-name-of-the-app`

To see what is running do `docker stack ls`, to see the services `docker stack services the-name-of-the-app` and to see all containers accross all nodes we do `docker stack ps counter`.

Recomended way to adjust replicas is to edit the `docker-compose.yml` file. Running then the old command for deployment will update it: `docker stack deploy -c docker-compose.yml the-name-of-the-app`.

To remove everything just do `docker stack rm counter`.


## Docker Volumes

In order to create a Docker Volume we simply run `docker run -it -p 8080:5000 -v ${PWD}:/app -w "/app" mcr.microsoft.com/dotnet/sdk:5.0`. If we are done with that we can do `docker ps -a` to list all running containers / volumes and then `docker rm 5b` we just put `-v` if we want to remove the source folder - however since we linked the source folder customly this won't delete it.