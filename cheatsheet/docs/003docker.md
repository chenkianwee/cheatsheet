# Docker Commands

Check for running container
```
$ sudo docker ps
```

Check all container including stopped container
```
$ sudo docker ps -a
```

Build a docker image, -t allows you to specify Name and optionally a tag in the ‘name:tag’ format
```
$ sudo docker build -t yourName/ImageName:tag
```

Login to Docker Hub
```
$ sudo docker login
```

Push build image to Docker github
```
sudo docker push yourName/ImageName:tag
```

purge the system to free up space, this is useful to clean the /var/lib/docker/overlay2 folder
```
sudo docker system prune -a 
```

check how much space docker is using
```
sudo docker system df
```
## Container
Go into the container
```
$ sudo docker exec -it containername bash
```

Go into the container as root user
```
$ docker exec -u root -it containername bash
```

Copy file from container to host
```
sudo docker cp containername:/file/path/in/container /file/path/in/host
```

Copy file from host to container
```
$ sudo docker cp /file/path/in/host containername:/file/path/in/container
```

Restart a container
```
$ sudo docker restart containername
```

Start a container
```
$ sudo docker start containername
```

Stop a container
```
$ sudo docker stop containername
```

Run a container, will grab the image specified and set it up. -d runs the container in the background.
```
$ sudo docker run -d --name name repository/ImageName:tag
```

Remove a container. -v remove volume associated with the container
```
$ sudo docker rm containername -v
```

## Volume
Remove a volume
```
sudo docker volume rm volume_name
```

List all volumes
```
sudo docker volume ls
```

purge the volume to free up space
```
sudo docker volume prune -a 
```

## Network
Remove a network
```
sudo docker network rm network_name
```

List all networks
```
sudo docker network ls
```

Create a network
```
sudo docker network create network_name
```

purge the network to free up space
```
sudo docker network prune -a 
```

## Image
List all images
```
sudo docker image ls
```

Remove image
```
sudo docker image rm imageName
```

Download an image
```
sudo docker image pull imageName
```

Download and save Docker image to a tar file with this command.
```
sudo docker save fraunhoferiosb/frost-server:latest > frost-server.tar
```

Load the image with this command.
```
sudo docker load -i frost-server.tar
```
## Build an image for multiple platform
- https://docs.docker.com/build/building/multi-platform/#strategies

### Using QEMU

1. Install QEMU (https://docs.docker.com/build/building/multi-platform/#qemu)
    ```
    sudo docker run --privileged --rm tonistiigi/binfmt --install all
    ```
2. build the image (https://docs.docker.com/build/building/multi-platform/#simple-multi-platform-build-using-emulation)
    ```
    sudo docker build --platform linux/amd64,linux/arm64 -t chenkianwee/yun2inf:0.0.x .
    ```
3. push the image to dockerhub
    ```
    sudo docker push chenkianwee/yun2inf:0.0.x
    ```