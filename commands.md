## Get Started

```sh
docker image ls -a
docker image rm $(docker image ls -a -q)   # Remove all images from this machine

docker container ls -a
docker container rm $(docker container ls -a -q)         # Remove all containers
```

### Containers

```bash
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```

### Services

```sh
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```


## Mac OS stuff

```sh
brew cask install virtualbox

docker-machine create --driver virtualbox myvm1

docker-machine env myvm1
# export DOCKER_TLS_VERIFY="1"
# export DOCKER_HOST="tcp://192.168.99.100:2376"
# export DOCKER_CERT_PATH="/Users/wbzyl/.docker/machine/machines/myvm1"
# export DOCKER_MACHINE_NAME="myvm1"

# Run this command to configure your shell:
eval $(docker-machine env myvm1)

docker-machine ls
# NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
# myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.12.1-ce
# myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.12.1-ce
```
