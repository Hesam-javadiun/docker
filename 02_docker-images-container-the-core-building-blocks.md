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
