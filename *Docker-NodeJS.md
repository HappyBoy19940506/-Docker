```
# syntax=docker/dockerfile:1

FROM node:12.18.1

ENV NODE_ENV=production / development

WORKDIR /app




```
** .dockerignore**
```
1. .dockerignore

.dockerignore
    ├── *.log
    ├── -dockerignore
    ├── -env
    ├── -git
    ├── Dockerfile
    ├── cicd
    └── node_modules

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

    
    
```

