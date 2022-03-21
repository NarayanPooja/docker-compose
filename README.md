Deploying any 3-tier application

We are going to deploy a 3-tier application using docker-compose First task would be to choose application. I’ll deploy PHPMyAdmin web application with MySQL as database and nginx as reverse proxy.

Creating docker-compose.yml

version: '2'
networks:
  net:
     driver: bridge
services:
  php:
     networks:
       - net
     image: phpmyadmin/phpmyadmin
     deploy:
       resources:
         limits:
           cpus: '0.5'
           memory: 50M
     container_name: php
     restart: on-failure
     links:
       - mysql:db
     depends_on:
       - mysql

  mysql:
    networks:
      - net
    image: k0st/alpine-mariadb
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 50M
    container_name: mysql
    restart: on-failure
    volumes:
      - ./data/mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=mydb
      - MYSQL_USER=myuser
      - MYSQL_PASSWORD=mypass

  nginx:
    networks:
      - net
    image: nginx:stable-alpine
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 50M
    container_name: nginx
    restart: on-failure
    ports:
      - "81:80"
    volumes:
      - ./nginx/log:/var/log/nginx
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - php



We need to create one nginx configuration file which will tell nginx container which we’ve setup as to work as reverse proxy, that what port to listen on and what service to show up on particular user request.

We’ll be creating a file named nginx.conf in same directory where docker-compose.yml is residing and content will be as follow:


worker_processes  1;
events {
  worker_connections  1024;
}
http {
  sendfile  off;
  server {
    listen 80;
 
    location / {
      proxy_pass  http://php;
      proxy_set_header Host $host;
      proxy_redirect     off;
    }
  }
}





Running services using docker-compose

Now, Run all the these services at once using a simple docker-compose command -> docker-compose up --build




Now, check the running applications using docker ps command, as shown below

![image](https://user-images.githubusercontent.com/90742840/158739077-bfc67a0f-d369-4027-afbf-c87bc11c3343.png)


Head toward the localhost at 81 port, we should land to MyPHPAdmin landing page

![image](https://user-images.githubusercontent.com/90742840/158738914-239daa91-0793-4ccd-8d84-27486220a631.png)

And now we have completed the task
