*Template dev.Dockerfile for React -dev env 前端后端都要写一个dockerfile*
* dev env 无nginx web server，但是 有测试，有挂载，有同步
```
# syntax=docker/dockerfile:1

# pull the official base image,choose a stable version, alias as build
FROM node:12.18.1 AS build

# running in a development environment by default
ENV NODE_ENV=development

# set working direction
WORKDIR /app

# Adding node_modules/bin to the PATH environment variable  - make sure node exectable command can run smoothly
ENV PATH /app/node_modules/.bin:$PATH


# copy application dependencies
COPY ["package.json","package-lock.json*", "./"]

# install application dependencies
RUN npm install --development

# copy app source code
COPY . .

EXPOSE 3000
# Expose specific ports,e.g. 
# React-3000 ExpressJS-9000 Nginx-80 MongoDB-27017 MySQL-3306

# start app , see package.json inside for details
CMD ["npm", "start"]


```
*Template docker-compose.yml*
```
version: "3.7"
services:
    client:
        #image: mhart/alpine-node:6.8.0
        build: 
            context: ./client/
            dockerfile: dev.Dockerfile
        restart: always
        ports:
            - "3000:3000"
        working_dir: /client
        volumes:
            - ./client:/client
            - /client/node_modules
        environment:
            - NODE_ENV=development
            - CHOKIDAR_USEPOLLING=true
            - CI=true
        networks:
            - my-react-app
    api:
        #image: yourdockHub/tagname
        build:
            context: ./api/
            dockerfile: dev.Dockerfile
        restart: always
        ports:
            - "9000:9000"
        # ExpressJS default port
        volumes:
            - ./api:/api
            - /api/node_modules
        environment:
            - NODE_ENV=development
            - CHOKIDAR_USEPOLLING=true
            - CI=true
  # We used the CI=true flag to run all our tests only once, because some test runners (e.g. Jest) would run the tests in watch mode and thus would never exit the process
        depends_on:
            - mongodb
        networks:
            - my-react-app
 mongo:
    image: mongo
    restart: always
    volumes:
        - ./mongodb_data:/data/db
    networks:
        - my-react-app
    environment:
        MONGO_INITDB_ROOT_USERNAME: root
        MONGO_INITDB_ROOT_PASSWORD: example

 mongo-express:
        image: mongo-express
        restart: always
        ports:
            - 8081:8081
        depends_on:
            - mongo
        networks:
            - my-react-app
        environment:
            ME_CONFIG_MONGODB_ADMINUSERNAME: root
            ME_CONFIG_MONGODB_ADMINPASSWORD: example
            ME_CONFIG_MONGODB_URL: mongodb://root:example@mongo:27017/

# 如果用docker exec -it id bash方法进入mongodb container，必须用用户名密码登入，否则会unauthorized.
# root@06e8e28840bb:/# mongo admin -u root -p example
# 进入后可以用 db.runCommand("connectionStatus")查看用户
# >use mydb 创建database， >db 当前db  >show dbs 当前数据库必须要有数据才能显示，所以， db.movie.insert({"name":"tutorials point"})

******
注意 数据库的volume挂载问题，每次运行容器都会保存db运行的数据。不停累加，如果配置变化，会导致access denied。要清空挂载目录才行。
******
            
networks:
    my-react-app:
        driver: bridge
        #default is bridge mode

```

     
*Docker run command*
```
docker run 
    -it # 't'will show colored some output,e.g. warning,'i' if image has layer which contians CMD ["/bin/sh" + nothing]bash commoand.it'll stop,and wait for input, ref: alpine:latest
    --rm    # delete container after stoping running
    -v ./${PWD}:/app    #current folder mount to /app/xxxx 
    -v /app/node_modules  # syncho the node_modoules inside and outside always (node_modules stores all packages downloaded by node)
    -p 3001:3000 #port mapping
    -e CHOKIDAR_USEPOLLING=true # used in react-app,mostly like the debug mode in Flask ,will polling scan the change and hot reload
    dockerhub-ID:tagname
```



***--------------------------------------------------------------------------------------------------------------------------------------***

*Best Practise*

**1. add.dockerignore**

**2. RUN command && command**

**3. Multi stage images** //因为一层一层运行，multi-stages可以parallel运行,if enable BuildKit tech.



```
1. .dockerignore

.dockerignore
    ├── *.log
    ├── .dockerignore
    ├── .env
    ├── **/.git
    ├── .gitignore
    ├── Dockerfile
    ├── **/.DS_Store
    ├── build
    └── **/node_modules   # anyway we build npm install will creat this, so ignore this folder when copy 

```
```
2、package.json

每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

负责 管理 整个项目用到的 依赖包 列表配置 以及 项目打包的一些脚本命令 scripts 配置

```
```
3. 简单明了package-lock.json作用
https://juejin.cn/post/7008886710134112264

```


*. common React File Structure*
```
react-app
├── package-lock.json // 锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致,对整个文件的描述,为的是让开发者知道只要你保存了源文件，到一个新的机器上、或者新的下载源，只要按照这个package-lock.json所标示的具体版本下载依赖库包，就能确保所有库包与你上次安装的完全一样，它是npm install自动生成的一文件
├── package.json // 对整个应用程序的描述,应用名称,版本号,一些依赖包,以及项目的启动,打包，测试配置，锁定大版本
├── public
│   ├── favicon.ico  // icon图标
│   ├── index.html   // 主页面,首页模板
│   └── manifest.json // 定义成app应用的方式来使用,快捷方式的图标,可以配置icons，定义快捷方式的图标,定义快捷方式跳转的网址到哪里,主题颜色,用于指定应用的显示名称、图标、应用入口文件地址及需要使用的设备权限等信息，类似于android里面的manifest.xlm配置文件
├── README.md // 说明文档
└── src       // 源码目录
    ├── App.css // App应用组件的样式
    ├── App.js  // App应用组件的逻辑代码,构成一个react组件的基本组成部分
    ├── App.test.js // App自动化测试文件
    ├── index.css   // 首页入口index的样式
    ├── index.js    // 整个程序运行的入口文件
    ├── logo.svg    // 图标,资源
    └── serviceWorker.js // 引入这个是为了帮助我们借助网页去写手机app应用这样的一个功能,如果上传到https协议的服务器上,在断网的情况下,依然可以看到之前的页面
    
 └── build //打包后的目录，里面放的是转译后的代码，可运行在浏览器上

    
    
```

*Template for Vue.js*

```
https://mherman.org/blog/dockerizing-a-vue-app/
```
