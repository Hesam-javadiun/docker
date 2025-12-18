# Docker Images and Containers the Core Building Blocks


## Images and Containers What and Why 

* container 
  * contain application and dependencies 
  * running unit of software 

* images
  * templates/blueprint for container 
  * contain code + required tools/runtimes

we have this split or separation because one image, multiple containers

* image (nodejs app code, nodejs environment)
  * container one running node js app
  * container two running node js app
  * container three running node js app

  
for example a node js application we can defined it once (the image ) but run it multiple times on different machines and different servers 


the image is sharable package with all setup instructions and all the code 

the container will be the concrete running instance of such an image

we run containers based on image this is core fundamental concept

## Using and Running External (Pre-Built)

we need an image?

* we can using an existing, pre-build image 
    * a great source for that getting image `docker hub`

we can find the node image in docker which developed and maintained by node team 

```cmd
docker run node
```

this command will use node image (which we can find that image in docker hub) and it will utilize it to created a so called container based on this image


> images container setup code the environment , in this case it contain node installation 

by running this command first it will got an error (for not finding node image locally) then it go itself to download node image from docker hub and run the container


by default the container is isolated from the surrounding environment and just because there might be some interactive shell


and in the end the container is running 
```cmd
docker ps -a
```
`ps` stands for processing `-a` flag shows all container 


we can also run this command

```cmd
docker run -it node
```

by adding `-it` flag we are telling docker we wanna expose an interactive session from inside the container to our hosting machine, 

and in the end we going to that node interactive terminal 


images is behind the scenes, is hold all the logic and all the code a container needs and then we create an instance of an image with `run` command 

## Our Gaol a NodeJS App

typically you would pull into your official base image and then add your  code on top of that to execute your code with that image 


## Building Our Own Image with DockerFile  

we need to create a new file `Dockerfile`( this is special name which will be identified by Docker) 

>to get best possible support we recommend that is visual studio code install Docker extension 


this file will contain the instruction for Docker that we wanna execute when we build our own image, so it contains setup instructions for our own image and typically here you start with FROM

* `FROM` keyword in docker file allows you to build your image up on another base image,

>theoretically you could build a docker image from scratch but you always some kind of operating system layer in there


```Dockerfile

FROM node 

COPY . .

```
after the `COPY` keyword we must initialize two path `.` and `.` .

first path is the path of outside the container, the image where path files leave that should be copied into that image


the second dot is the path inside of image where those files should be stored every image and for also every container based on that image has its own internal file system which is totally detach from your file system

in the end `COPY . /app` by executing docker file all files in the first path will copied into app folder (that folder will be created in the container) in container 


```Dockerfile

FROM node 

COPY . /app

RUN npm install
```

by default all command execute on working directory of your docker container and image and by default that working directory is the root folder in that container file system, since we copy the code in `/app` folder we want run npm command in that app folder and the convinient way telling docker all commands should execute in specific folder is 


```Dockerfile 

FROM node 

WORKDIR /app

COPY . ./

RUN npm install
```
>by adding `WORKDIR` path we can add `./` in the second path in copy command and its equal to `/app` because `./` is current working directory of container 

the last structure we want to run our container 

```Dockerfile
FROM node 

WORKDIR /app

COPY . ./

RUN npm install

RUN node server.js
```

well the `RUN node server.js` is not correct all these instruction to docker for setting up the image Now keep in mind the image should be the template for the container, the image is not what you run in the end you run container based on an image 

***so by executing this command `RUN node server.js` we would start running container in the image ***

we have another instruction that's the `CMD` stands for Command, the difference to `RUN` is that this will now not be executed when the image is created, but when a container is started based on the image 

```Dockerfile
FROM node 

WORKDIR /app

COPY . ./

RUN npm install

CMD ["node", "server.js"]
```

> if you don't specify a CMD, the CMD of that base image will be executed, with no base image and no CMD, you'd get an error.


there is one thing left our node server listen to port 80 

```jsx
app.listen(80);
```

the docker container is isolated, its isolated from our local environment, its has its own internal network and when we listen to port 80 in node application inside of our container does not expose that port to our local machine so we won't be able to listen on the port just because something's listening inside of a container therefor in the docker file after setting everything up we can EXPOSE instruction 

```Dockerfile
FROM node 

WORKDIR /app

COPY . ./

RUN npm install

CMD ["node", "server.js"]
```

