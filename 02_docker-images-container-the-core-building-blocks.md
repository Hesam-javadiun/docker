
<!-- omit from toc -->
#  Docker Images and Containers the Core Building Blocks

- [1. Images and Containers What and Why](#1-images-and-containers-what-and-why)
- [2. Using and Running External (Pre-Built)](#2-using-and-running-external-pre-built)
- [3. Our Gaol a NodeJS App](#3-our-gaol-a-nodejs-app)
- [4. Building Our Own Image with DockerFile](#4-building-our-own-image-with-dockerfile)
- [5. Running a Container Based on Our Own Image](#5-running-a-container-based-on-our-own-image)
- [6. Images are Read-Only](#6-images-are-read-only)
- [7. Understanding Image Layers](#7-understanding-image-layers)


## 1. Images and Containers What and Why 

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

## 2. Using and Running External (Pre-Built)

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

## 3. Our Gaol a NodeJS App

typically you would pull into your official base image and then add your  code on top of that to execute your code with that image 


## 4. Building Our Own Image with DockerFile  

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

## 5. Running a Container Based on Our Own Image 

first of all we wanna create an image based on our docker file, that `build {path}` command does 

```
docker build .
```
if it build successfully in the end it will give you an id where you run container based on that image
```
docker run {image-id}
```

if execute this command if won't finish, it will keep running the reason for that is this line in docker file 

```Dockerfile
CMD ["node", "server.js"]
```
now a problem arise if you visit the `localhost:80` you won't see the website

lets shut down that container
let see the containers they are processing
```
docker ps
```
`docker ps` without `-a` you see only the running containers 

by running the command 

```
docker stop {container-name}
```
container will shout down

this docker file is for documentation purposes, in order to work we need to add `-p` flag to run command

```
docker run -p 3000:80 {image-id}
```
`p` stands for publish and this allow us to tell docker under which local port on our machine , this internal `EXPOSE 80` docker container-specific port should be accessible, `-p 3000:80` 80 is the docker container expose and the 3000 is the local port on our machine we tell docker to expose 


## 6. Images are Read-Only

you must understand if you have modified some code, the changes won't reflect in the container that is running, 

we instruct docker to copy everything inside `app` folder into the container file image system, 

that image is snapshot of my source code and are readonly, ***an image is really closed template in the end ***


## 7. Understanding Image Layers

images are layer based when you build an image only the instructions where something changed and the instructions there after are ***re-evaluated *** 

we have changed the code and rebuild an image , after we didn't any changes to source code then run build command we would see the it will finish super fast like a quarter of a second

docker basically recognized that for all these instructions and use the cached for results for each instruction

every instruction represents a layer in your docker file 


* an image
  * instruction #4 : image layer4
  * instruction #3 : image layer3
  * instruction #2 : image layer2
  * instruction #1 : image layer1

once the image has been executed the image is locked in and code in there can't change 


if we run a container based on an image, that container basically adds a new extra layer on top of the image which is that running application that running code the result of executing command in docker file 

```Dockerfile
CMD ["node", "server.js"]
```

* a container 
  * run command instruction #4 : container layer
  * instruction #4 : image layer4
  * instruction #3 : image layer3
  * instruction #2 : image layer2
  * instruction #1 : image layer1

this add the final layer which only becomes active once you run an image as a  layer 

all the instructions before that final instruction are already part of the image though as separate layers


this layer architecture exist to speed up the creation of images since docker only rebuilds and re-executes what needs to be executed 


we can do a tiny optimization potential for our docker file 
consider this 

```Dockerfile
FROM node 

WORKDIR /app

COPY . ./

RUN npm install

CMD ["node", "server.js"]
```
we can optimize it to this 

```Dockerfile
FROM node 

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . ./

CMD ["node", "server.js"]
```
duo to docker layer architecture based on the fist docker (because `RUN npm install` is under the `COPY . ./`  changing source code makes npm install command invalidated and run again on every change ) changing the package.json, does ***not*** lead to re-executing `RUN npm install` so that's why we add `COPY package.json` to ensure the to have the latest package.json file 
and move up the `RUN npm install` instruction and move down the `COPY . ./` instruction,

now by changing the source code npm run install will not invalidated and npm run install will executed 
 


