*Template prod.Dockerfile for React -prod env 前端后端都要写一个dockerfile* 
* prod env 有nginx，但是不挂载，不同步, 无测试
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

```
*nginx dockerfile*
```
# production environment -set nginx as web server

FROM nginx:1.17

#npm build will create a folder named build to cache static files, copy build folder into nginx container cache folder

COPY --from=build /home/node/code/build /usr/share/nginx/html

COPY ./default.conf /etc/nginx/conf.d/default.conf


EXPOSE 80

```
*Template docker-compose.yml*
```
version: "3.7"
services:
    nginx:
      depends_on:
        - api
        - client
      restart: always
      build:
        dockerfile: prod.Dockerfile
        context: ./nginx
      ports:
        - "80:80"
      networks:
            - my-react-app
      
    client:
        #image: mhart/alpine-node:6.8.0
        build: 
            context: ./client/
            dockerfile: prod.Dockerfile
        restart: always
        ports:
            - "3000:3000"
        working_dir: /client
        environment:
            - NODE_ENV=production
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

        environment:
            - NODE_ENV=production
 
  # We used the CI=true flag to run all our tests only once, because some test runners (e.g. Jest) would run the tests in watch mode and thus would never exit the process
        depends_on:
            - mongodb
        networks:
            - my-react-app
    # mongodb just a service name,you can customize        
    mongodb:
        image: mongo
        restart: always
        #container_name: mongodb
        volumes:
            - ./mongodb_data:/data/db
        ports:
            - 27017:27017
        environment:
            MONGO_INITDB_DATABASE: my-database-name
            MONGO_INITDB_ROOT_USERNAME: root
            MONGO_INITDB_ROOT_PASSWORD: password
        networks:
            - my-react-app
networks:
    my-react-app:
        driver: bridge
        #default is bridge mode








```
*Nginx conf settings for react *
```
----nginx.conf
upstream api {
  server api:9000;
}

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
  
   location /api {

      rewrite /api/(.*) /$1 break;
      proxy_pass http://api;
   # how to redirect to api back-end, depends on the source code
  }

}

```

***--------------------------------------------------------------------------------------------------------------------------------------***


*1.如何理解Nginx, WSGI, Flask之间的关系*
```
https://blog.csdn.net/lihao21/article/details/52304119
```

*2.nginx实现80端口重定向至443（http跳转https)*
