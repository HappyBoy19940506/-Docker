**--------------------how to wirte a dockerfile------------------------------------**


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

比如  docker run -p 80:80 -v 'pwd':'usr/share/nginx/html image-name:1.0 
这样的话，外部pwd 文件夹  内部的 html文件夹会形成映射， 两边同时储存，同时变化。

```
```
 container创建后 如果没有一个阻塞进程（比如监听），就会自动关闭running状态，docker -d 可以将阻塞进程后台显示。
  这就是问什么 cmd里面写 run
```

