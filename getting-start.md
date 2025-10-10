# Docker

## What is docker

docker is a container technology : a tool for creating and managing containers

* container 
  * a container is a standardized unit of software 
  * a package of code and dependencies to run that code (e.g NodeJS code + the NodeJS runtime)

the same container always yields the exact same application and execution behavior! no matter where or by whom it might be executed

the container is isolated thing that can run it self without problem

support for container is built into modern operating systems

and Docker simplifies the creation and management fo such containers


## Why Docker and Container 

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


## Virtual Machine Vs Docker Containers

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