Deploying any 3-tier application

We are going to deploy a 3-tier application using docker-compose First task would be to choose application. I’ll deploy PHPMyAdmin web application with MySQL as database and nginx as reverse proxy.

1. Creating docker-compose.yml

![image](https://user-images.githubusercontent.com/90742840/159202496-15a217b7-3d92-4f87-9215-9e3b95c9046e.png)



We need to create one nginx configuration file which will tell nginx container which we’ve setup as to work as reverse proxy, that what port to listen on and what service to show up on particular user request.

We’ll be creating a file named nginx.conf in same directory where docker-compose.yml is residing and content will be as follow:





![image](https://user-images.githubusercontent.com/90742840/159202644-46e501ab-eb72-409a-a35b-b45d8722bb98.png)



Running services using docker-compose

Now, Run all the these services at once using a simple docker-compose command -> docker-compose up --build




Now, check the running applications using docker ps command, as shown below

![image](https://user-images.githubusercontent.com/90742840/158739077-bfc67a0f-d369-4027-afbf-c87bc11c3343.png)


Head toward the localhost at 81 port, we should land to MyPHPAdmin landing page

![image](https://user-images.githubusercontent.com/90742840/158738914-239daa91-0793-4ccd-8d84-27486220a631.png)

And now we have completed the task
