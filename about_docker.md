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

### Port publishing on containers

<img width="1512" alt="Screenshot 2023-09-19 at 1 02 26 AM" src="https://github.com/iamskp99/Fundamentals-of-Database-Engineering/assets/42648568/140ab394-6ad2-4b26-bbb1-58c01470259f">

### Inspect container

<img width="1512" alt="Screenshot 2023-09-19 at 1 07 40 AM" src="https://github.com/iamskp99/Fundamentals-of-Database-Engineering/assets/42648568/58a2e04f-59a2-4b99-812f-3f9e2e01d048">

### To pass environment variable while running a docker container just insert an *-e* flag


