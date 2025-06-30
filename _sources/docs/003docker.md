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
$ sudo docker push yourName/ImageName:tag
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
$ sudo docker volume rm volume_name
```
List all volumes
```
$ sudo docker volume ls
```

## Network
Remove a network
```
$ sudo docker network rm network_name
```
List all networks
```
$ sudo docker network ls
```
Create a network
```
$ sudo docker network create network_name
```

## Image
List all images
```
$ sudo docker image ls
```
Remove image
```
$ sudo docker image rm imageName
```
Download an image
```
$ sudo docker image pull imageName
```
Download and save Docker image to a tar file with this command.
```
$ sudo docker save fraunhoferiosb/frost-server:latest > frost-server.tar
```
Load the image with this command.
```
$ sudo docker load -i frost-server.tar
```
## Build an image for multipleOS
- https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/

### Old Manifest way
- build and push your linux-amd64 image
```
sudo docker build . -t chenkianwee/yun2inf:0.0.8-linuxamd64
sudo docker push chenkianwee/yun2inf:0.0.8-linuxamd64
```

- build and push your linux-arm64 image
```
sudo docker build . -t chenkianwee/yun2inf:0.0.8-linuxarm64
sudo docker push chenkianwee/yun2inf:0.0.8-linuxarm64
```

- consolidate the image with manifest and push it to the repository
```
sudo docker manifest create chenkianwee/yun2inf:0.0.8 --amend chenkianwee/yun2inf:0.0.8-linuxamd64 --amend chenkianwee/yun2inf:0.0.8-linuxarm64
sudo docker manifest push chenkianwee/yun2inf:0.0.8
```

### Not working on my machine
Build an image of multiple OS/Arch
```
sudo docker buildx build --push --platform linux/arm64 --tag chenkianwee/yun2inf:0.0.8 .

sudo docker buildx build --push --platform linux/amd64 --tag chenkianwee/yun2inf:0.0.8 .
```
