# Docker commands

```
docker run nginx
```
This command will start a container of nginx. If the image is not present , it will pull the image from docker-hub and then start the container.


```
docker ps
```
It will list all the container

```
docker stop container_id
```

```
docker rm container_name
```

This will delete the container. Once the container is deleted , then it will return the name of the container.

```
docker images
```

This will return all the images currently available

```
docker rmi image_name
```

This will remove the image.

```
docker pull image_name
```

This will pull the image from docker-hub but it will not run it.

