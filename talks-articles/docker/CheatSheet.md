## Docker CheatSheet

### Docker Basics

* running docker service

```
docker daemon
```

* search for image available by name

```
docker search $IMAGE_NAME_PART
```

* pull a docker image from docker hub

```
docker pull $IMAGE_NAME_WITH_WITHOUT_VERSION
```

* current alive docker instances

```
docker ps
```

* container IDs of current alive docker instances

```
docker ps -q
```

* checking all ran docker instances

```
docker ps -a
```

* will download, run and exit

```
docker run IMAGENAME
```

* run in interactive mode

```
docker run -i IMAGENAME
```

* run with pseudo-tty mode

```
docker run -t IMAGENAME
```

* working within docker proper

```
docker run -it IMAGENAME
```

* give your name to docker running instances

```
docker run --name INSTANCE_NAME -it IMAGENAME
```

* run it in detach mode, useful for service containers

```
docker run -d IMAGENAME
```

* attach to detached run

```
docker attach CONTAINER_ID_FROM_PS
```

* info on container

```
docker inspect CONTAINER_ID_FROM_PS
```

* history of commands run on container

```
docker history CONTAINER_ID_FROM_PS
```

* stop a running container

```
docker stop CONTAINER_ID_FROM_PS
docker stop INSTANCE_NAME
```

* to kill a docker instance

```
docker kill CONTAINER_ID_FROM_PS
docker kill $(docker ps -q)
```

* to clean-up instances created for docker run

```
docker rm CONTAINER_ID_FROM_PS
docker rm $(docker ps -aq)
```

* to delete an image by name

```
docker rmi $CONTAINER_NAME
```

* to run command (say 'ls /') within container and spit output

```
docker exec alpine ls /
```

---

### Volumes

* it will give you a filesystem inside container
> a volume to live till this container (goes away when removed, not stopped), not shared
```
docker run -it -v /someVolume alpine
```

* when you add a file to an image, its copied not referenced

```Dockerfile
FROM alpine
ADD file1 /someVolume/file
VOLUME /someVolume
CMD ["/bin/sh"]
```

* to have access to docker host filesystem, will map host's $PWD/myVolume

```
docker run -it -v $PWD/myVolume:/someVolume alpine
```

* to mark mounting host's filesystem as volume READONLY

```
docker run -it -v $PWD/myVolume:/someVolume:ro alpine
```

* can map container bash\_history to host file to keep a tab

* mapping volumes from one container to other, named containers ease else need to fetch name

```
docker run --name templateImage -it -v $PWD/myVolume:/someVolume alpine
docker run -it --name someImage --volumes-from templateImage alpine
```

* is like a shorthand to take backup of mounted volume in templateImage

```
docker run --rm --volumes-from templateImage -v $(pwd):/backup alpine tar cvf /backup/some.tar /someVolume
```

---
---
