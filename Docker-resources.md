**--------------------docker command------------------------------------**
```
docker image ls
```

```
docker image rm [image name]
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
docker build -t [new-image-name] .
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
print log of container

docker log --tails 100 container_name

```

```
Delete all running and stopped containers

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
**--------------------how to wirte a dockerfile------------------------------------**

```
RUN [command]
相当于在 命令行中运行 什么
```
```
ADD [host_file/folder]     [destination_inside_docker]
拷贝文件/文件夹/url。 到 容器里面去，并且会自动解压
```
```
COPY [host_file/folder]     [destination_inside_docker]
拷贝文件/文件夹/url。 到 容器里面去，但是 和add 不同的是，不会自动下载或者解压
```

```
EXPOSE
决定了docker中的该container对外开放了 哪个端口。

```
```
ENV
设置container内的环境变量
ENV HOME_PATH /Users/user
```

```
CMD
每个dockerfile里面之内只能存在一个cmd命令，用作容器启动之后 执行什么操作.
比如flask，肯定要执行 cmd python3 app.py  //// CMD ["python", "./src/app.py"]
如果有两个，后面的会覆盖前面的。 如果在shell中重新定义，也会使得dockerfile中的cmd失效。
```

```
ENTRYPOINT
  作用和cmd一样 ，一个dockerfile里面只能有一个entrypoint。
  但是 如果同时写了cmd和entrypoint， 谁在后面谁生效。
  
```

```
WORKDIR
 为 RUN, CMD, ENTRYPOINT, COPY and ADD instructions 设置目录，也就是说 进入容器之后 在哪里
 
```

```
VOLUME ["/var/lib/mysql"]
容器中的/var/lib/mysql目录 会在容器运行时 自动复制到 宿主机的  /var/lib/docker/volumes下保存起来

但是一般不写在dockerfile里，一般在 docker run -p --name xxxxx 中使用-v 来临时决定存在哪里

```

