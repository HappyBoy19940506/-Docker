*Template prod.Dockerfile for React -prod env*
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
*Tempalte prod.Dockerfile*
```
# syntax=docker/dockerfile:1

FROM node:12.18.1

# logging is kept to a minimum, essential level , more caching levels take place to optimize performance

ENV NODE_ENV=production
WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production 

COPY . .

CMD ["npm","run", "build" ]

```

***--------------------------------------------------------------------------------------------------------------------------------------***


*1.如何理解Nginx, WSGI, Flask之间的关系*
```
https://blog.csdn.net/lihao21/article/details/52304119
```

*2.nginx实现80端口重定向至443（http跳转https)*
