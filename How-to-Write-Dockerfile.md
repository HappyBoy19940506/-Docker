**--------------------how to wirte a dockerfile------------------------------------**
```
FROM

不多说了

```
```
WORKDIR
 为 RUN, CMD, ENTRYPOINT, COPY and ADD instructions 设置目录，也就是说 进入容器之后 自动会cd在哪里
 
```


```
RUN [command]
相当于在 容器build过程中 命令行中执行什么。。。
```
```
ADD 【不常用】 [host_file/folder]     [destination_inside_docker]
拷贝文件/文件夹/url。 到 容器里面去，并且会自动解压
```
```
COPY 【更常用】 [host_file/folder]     [destination_inside_docker]
拷贝文件/文件夹。 到 容器里面去，但是 和add 不同的是，不会自动下载或者解压
```

```
EXPOSE
决定了docker中的该container对外开放了 哪个端口。
The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. 

You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.
****************************************************************************************************
The EXPOSE instruction does not actually publish the port. 

It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. 

To actually publish the port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all exposed ports and map them to high-order ports.
****************************************************************************************************
```
```
ENV
设置container内的环境变量
ENV HOME_PATH = '/Users/user'
调用: $HOME_PATH
```
```
ARG
相比较于env（ 在build 和 run都一直生效  ）

ARG 只在build时候生效，build完成后就消失了，run的时候是没有的

```
```
CMD
1.相当于在 容器run过程中 命令行中执行什么。。。一定要写 阻塞式指令，否则容器执行完 生命周期结束，docker ps 就查不到了，会自动 停止运行。
2。每个dockerfile里面之内只能存在一个cmd命令，用作容器启动之后 执行什么操作.
比如flask，肯定要执行 cmd python3 app.py  //// CMD ["python", "./src/app.py"]
3.如果有两个，后面的会覆盖前面的。 
4.如果在shell中重新定义，比如 docker run echo xxx，也会使得dockerfile中的cmd失效。
```

```
ENTRYPOINT
  1.作用和cmd一样 ，一个dockerfile里面只能有一个entrypoint。
  2.如果在shell中重新定义，比如 docker run echo xxx，entrypoint无影响还是听 entry point的。
  
  但是 如果同时写了cmd和entrypoint 1.如果2者都是 jason format，他们会拼接运行，一起运行。 规则： entrypoint在前，cmd再后，**拼接运行**
                                2.如果 entry point不是 jason format，直接 覆盖其他所有cmd。 以entrypoint为准运行。
 
```

```
VOLUME ["/var/lib/mysql"]
容器中的/var/lib/mysql目录 会在容器运行时 自动复制到 宿主机的  /var/lib/docker/volumes下保存起来

但是一般不写在dockerfile里，一般在 docker run -p --name xxxxx 中使用-v 来临时决定存在哪里

比如  docker run -p 80:80 -v 'pwd':'usr/share/nginx/html image-name:1.0 
这样的话，外部pwd 文件夹  内部的 html文件夹会形成映射， 两边同时储存，同时变化。

```
```
 container创建后 如果没有一个阻塞进程（比如监听），就会自动关闭running状态，docker -d 可以将阻塞进程后台显示。
  这就是问什么 cmd里面写 run
```

```

ONBUILD xxx

比如ONBUILD ENV VARIBLE = 100
当前dockefile创建镜像时，不会 触发 该onbuild命令。
但是 如果有别的dockerfile FROM了该镜像， 那么这个dockerfile 在build其镜像时候 就会触发 该命令 生成一个 值为100的参数。

```


```

SHELL

SHELL /bin/sh
SHELL CMD
SHELL /bin/bash

```
