<!-- omit from toc -->
# Docker
- [1. What is docker](#1-what-is-docker)
- [2. Why Docker and Container](#2-why-docker-and-container)
- [3. Virtual Machine Vs Docker Containers](#3-virtual-machine-vs-docker-containers)
- [4. Docker Setup Overview](#4-docker-setup-overview)
- [5. Getting your Hands Dirty](#5-getting-your-hands-dirty)

## 1. What is docker

docker is a container technology : a tool for creating and managing containers

* container 
  * a container is a standardized unit of software 
  * a package of code and dependencies to run that code (e.g NodeJS code + the NodeJS runtime)

the same container always yields the exact same application and execution behavior! no matter where or by whom it might be executed

the container is isolated thing that can run it self without problem

support for container is built into modern operating systems

and Docker simplifies the creation and management fo such containers


## 2. Why Docker and Container 

why do we need container in software development? 

in order to answer we need answer this why would we want ***independent, standardized application packages*** ? 
 


* Environment:
  * the runtimes
  * languages
  * frameworks you need for development 

we often have different development and production environments and we want to build and test in exactly the same environment as we later run our app (consider the scenario where we want to use node the specific version of node in our environment )

and 

we often have different development environment withing a team / company every team member should have the exactly same environment when working on the same project




usually we have two environment
* development
  * maybe we have more than one development environment
* environment



and we want reliability and reproducible environment

* we want to have exact some environment for the development and production this ensures that it works exactly as tested


* it should be easy to share a common development environment / setup with new employees and colleagues


and if you have multiple projects that may clashing tools/versions between different projects

when switching between projects, tool used in project A should not clash with tools used in project B 


## 3. Virtual Machine Vs Docker Containers

so machines running on our machines, virtual machines with virtual operating systems encapsulated in their own shell independent from our host operating system


with virtual machine you have your host operating system windows or macos or linux then on top of that, we install the virtual machine so a computer inside of our computer 


this virtual machine has its own operating system virtual OS (e.g. linux ) then in this virtual machine since its like a computer it's just virtually emulated,

we can install all the dependencies and libraries and the tools it needs and then move our source code 

in this scenario we have the same result as wit docker and containers(we have encapsulated environment where everything is locked in, and we could have multiple environment )

is this scenario where we use virtual machine works as docker and container but there are a couple of problems one of the biggest problems in the virtual operating system and in in general the overhead we have with multiple virtual machines every virtual machine is really like standalone computer a standalone machine running on top of our machine and waste a lot of space on your hard drive and tends to be slow(consumption memory, cpu ... )



virtual machine solution 
* pro's
  * separated environments
  * environment-specific configurations are possible 
  * environment configurations can be shared ad reproduced reliably 

* con's
  * redundant duplication, waste of space 
  * performance can be slow, boot times can be long 
  * reproducing on another computer/server is possible but may still tricky



docker help you build and manage "containers"
* your operating System 
  * OS built-in / Emulated Container Support (something docker take care of, emulate means reproduce the function or action of ...) 
  * we run a tool called the Docker engine on top of that 
  * Docker engine is lightweight small tool, we can spin up containers with it 
  * these container contain our code and runtimes our code needs like node.js
  * but don't contain a bloated operating system and its dependencies(they might have small operating system layer inside the container) 
  * other great thing about containers is that you can configure and describe with configuration file and you can then share that file with others so that they can recreate the container or 
  * you can also build the container into something which is called an image and share that image with others to ensure that everyone is able to launch that same container in your system on their system


* docker containers 
  * low impact on OS, very fast, minimal disk space usage
  * sharing, re-building and distribution is easy 
  * encapsulate apps/ environments instead of "whole machines"

* virtual machines
  * bigger impact on OS slower, higher disk space usage
  * sharing , rebuilding and distribution can be challenging 
  * encapsulate whole machines instead of just apps/environments



## 4. Docker Setup Overview


* macos 
* windows
  * if your system has minimum requirement you can install docker desktop
  * if you don't have the requirement, you need to install docker toolbox 
* linux
  * linux natively support the docker engine 






in the end you you have installed docker engine 
if you are able to install docker desktop you have ***daemon*** and ***cli***

there is a service called ***Docker Hub***
there is a tool called ***Docker Compose***


## 5. Getting your Hands Dirty 

we have created file with no extension `Dockerfile` 

```Dockerfile
FROM node:14

WORKDIR /app

COPY package.json

RUN npm install

COPY . . 

EXPOSE 3000

CMD ["node", "app.mjs"]
```


these docker is for the project with 3 files
`app.mjs`
```js
import express from "express";

import connectToDatabase from "./helpers.mjs";

const app = express();

app.get("/", async (req, res) => {
  res.send("<h2>Hi there!</h2>");
});

await connectToDatabase();

app.listen(3000);
```
`helpers.js`
```js
const connectToDatabase = async () => {
  const dummyPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve();
    }, 1000);
  });
  return dummyPromise;
};

export default connectToDatabase;
```
and a package.json
`package.json`
```json
{
  "name": "docker-complete",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://git-codecommit.us-east-1.amazonaws.com/v1/repos/docker-complete-guide"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```


that `Dockerfile` describe the container and is couple of instruction :
* we want to use NodeJS as base `image` (we want that NODEJS available inside our container)
* we have certain directory `/app` in the container 
* then copy the `package.json` file into our working directory 
* then run the command `npm install`
* copy the rest of the code 
* then we expose port 300 to the outside world
* then we execute the `app.mjs` with the node command 


