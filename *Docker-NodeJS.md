*Template for React -dev env*

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

# start app , see package.json inside for details
CMD ["npm", "start"]


```

```
dockercompose.yml

ssss
```

```
docker run 
    -it # 't'will show colored some output,e.g. warning,'i' if image has layer which contians CMD ["/bin/sh"]bash commoand.it'll stop,and wait for input
    --rm    # delete container after stoping running
    -v ${PWD}:/app    #current folder mount to /app/xxxx 
    -v /app/node_modules  # syncho the node_modoules inside and outside always (node_modules stores all packages downloaded by node)
    -p 3001:3000 #port mapping
    -e CHOKIDAR_USEPOLLING=true # used in react-app,mostly like the debug mode in Flask ,will polling scan the change and hot reload
    dockerhub-ID:tagname
```

*Template for React -prod env*
```
----nginx.conf
server {

  listen 80;

  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;

    # to redirect all the requests to index.html, 
    # useful when you are using react-router

    try_files $uri /index.html; 
  }

  error_page   500 502 503 504  /50x.html;

  location = /50x.html {
    root   /usr/share/nginx/html;
  }

}
my-app
│   Dockerfile.prod
│   .dockerignore    
│
└───nginx
      nginx.conf

```
```
name it as Dockerfile.prod
# syntax=docker/dockerfile:1

FROM node:12.18.1

# logging is kept to a minimum, essential level , more caching levels take place to optimize performance

ENV NODE_ENV=production or development 

WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production or development

COPY . .

CMD ["npm", "start"] or  CMD [ "node", "server.js" ]

```
*----------------------------------------------------------------*

*Template for Vue.js*

```
https://mherman.org/blog/dockerizing-a-vue-app/
```



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

