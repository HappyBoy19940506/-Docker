**--------------------docker command------------------------------------**
```
docker image ls
```

```
docker image rm [image name]


docker image rmi [image name] ----要清楚干净用rmi，因为image是分层build，如果用rm可能有底层image缓存没有删除
```

```
docker ps -a = docker container ls -a
```

```
docker ps = docker container ls 这种只显示正在运行的，不能显示全部
```

```
docker rm [containter ID]
```

```
docker build -t [new-image-name]:tagname . 
如果不加tag，那么每次build dockerfile会重写改image，如果你改了dockerfile，，再build同名的image就会覆盖旧版本。
```

```
docker run (-d)(--rm) -p 80:80 --name container_name image-name 
```

```
关于端口映射。
1. app.py 中设置的flask开放端口  === dockerfile中 EXPOSE的端口号。=== docker run -p host-port : container-port
2. 在  docker run -p host-port:container-port --name cotainer-name image_name 中。  右边 container-port端口号也要和上面2者保持一致。
3. 左边的host-port为宿主机开放端口，可以用在browser监听。但是是 多 对 一的映射关系，即 宿主机端口 只能 对应一个 docker端口， 但是多个宿主机端口可以 对应 同一个docker端口。

  比如 container1- 80:80,  container2- 80:81. 肯定是错误的，不可能让 浏览器既监听80又监听81
  
  但是。container1- 81:80  containr2- 80:80. 是可以的， 2个浏览器可以同时监听一个服务器发送的内容
```

```
docker run --rm -p 80:5000 --name webapp_1 myimage:1.0
    --rm  阻塞显示，中断后立即删除 该container
```

```
docker run -d -p 80:5000 --name webapp_1 myimage:1.0
    -d 不阻塞显示，需要到后台 docker ps -a来查看正在运行的该container
```

```
stop a running container:

Ctrl+C in CLI
docker stop container_name
docker kill container_name

```

```
print logs of container

docker logs --tail 100 container_name

```

```
容器运行后，如何进入容器里面玩，容器里直接改动会使得展现的东西立刻发生变化：

docker exec -it +containername bash


```

```
>>>>>>>>>>>>>>>>>>Delete all running and stopped containers<<<<<<<<<<<<<<<<<<<<<<<<<<<

docker container rm -f $(docker ps -aq)

```


```
image会自动成为一个 repo，后续同名的image会自动加入到同一个repo中，用tag来区别版本。

docker build -t DockerHubAccountID/repo(image):tag .

docker push docker-acc-Id/repo(image):tag

```

```
Retag a local image with a new image name and tag

docker tag myimage:1.0 myrepo/myimage:2.0

```

```
>>>>>>>>>>>>>>>>>>To delete all containers including its volumes use,<<<<<<<<<<<<<<<<<<<<<<<<<<<

docker rm -vf $(docker ps -aq)

```

```
>>>>>>>>>>>>>>>>>>To delete all the images<<<<<<<<<<<<<<<

docker rmi -f $(docker images -aq)

```
