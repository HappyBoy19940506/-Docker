*Template prod.Dockerfile for React -prod env 前端后端都要写一个dockerfile* 
```
# syntax=docker/dockerfile:1

# pull the official base image,choose a stable version, alias as build
FROM node:12.18.1 AS build

# logging is kept to a minimum, essential level , more caching levels take place to optimize performance
ENV NODE_ENV=production

# set working direction
WORKDIR /app

# Adding node_modules/bin to the PATH environment variable  - make sure node exectable command can run smoothly
ENV PATH /app/node_modules/.bin:$PATH


# copy application dependencies
COPY ["package.json","package-lock.json*", "./"]

# install application dependencies
RUN npm install --production 

# copy app source code
COPY . .

EXPOSE 3000
# Expose specific ports,e.g. 
# React-3000 ExpressJS-9000 Nginx-80 MongoDB-27017 MySQL-3306

# start app , see package.json inside for details
CMD ["npm","run", "build" ]


# production environment -set nginx as web server

FROM nginx:1.17

#npm build will create a folder named build to cache static files, copy build folder into nginx container cache folder

COPY --from=build /home/node/code/build /usr/share/nginx/html

EXPOSE 80


```
*Nginx conf settings for react *
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

```

***--------------------------------------------------------------------------------------------------------------------------------------***


*1.如何理解Nginx, WSGI, Flask之间的关系*
```
https://blog.csdn.net/lihao21/article/details/52304119
```

*2.nginx实现80端口重定向至443（http跳转https)*
