# What is Docker
Docker is containerization platform. Using this platform one can create and write their application and containerize it so that it can be shipped and roll out at any time to higher environment or mupliple environment with consistent outcome.

# Why to use Docker
Docker is open source technology platform helps in developing, shipping, and running applicationsto idempotent the application. It separate out your application from infrastructure needed to run the application. Dockerizing application helps you deploy the application in few minutes on multiple places with consistent experience and outcome hence its super reliable.
Docker is platform to help containerize application.

## What is Container
A container is way of packaging an application in a loosely isolated environment to provide the consistent results and help with creating multiple environment seamless. To run or use the application which is packaged into the container it has to be executed as image. To create container it needs platform and docker provide that platform to package the entire application in isolated unit.
A container is running version of image, runnable instance of an image. 

## What is container image
An image is light weight, standalone, executable package of software that includes everything needed to run an application  like application code, runtime, system tools, libraries and environment settings etc.
An image is very light weight because the packages it shares and refer the host operating system kernel and libraries, read only template with instruction to create container.
Image needs to be save/store at some place and docker provides that place known as registry. Docker Hub is a public registry that anyone can use, and Docker looks for images on Docker Hub by default. You can even run your own private registry if needed.

> Physical server => Virtualization (Hypervisor) => Conatinerization (Docker) => Images => Conatiner (your application, deamon)

# What is docker image
Docker images are built based on whats written inside the dockerfile. Dockerfile is set of instruction to reference within application and run as container.

# Docker File Syntax
Each docker file should follow the basic syntax:
> // syntax=docker/dockerfile:1 <br>
> FROM {ubuntu:22.04} <br>
> WORKDIR {/app}
> COPY {requirements.txt} {./}
> // install app dependencies <br>
> RUN apt-get update && apt-get install -y python3 python3-pip <br>
> RUN pip install flask==3.0.* <br>
> // install app <br>
> COPY hello.py / <br>
> // final configuration <br> setting up environment variable
> ENV FLASK_APP=hello <br>
> // port mapping. Expose Ports.<br>
> EXPOSE 8000 <br>
> // Finally, CMD instruction sets the command that is run when the user starts a container based on this image.
> CMD ["flask", "run", "--host", "0.0.0.0", "--port", "8000"]

### Syntax reference: https://docs.docker.com/build/concepts/dockerfile/#dockerfile-syntax ###

# Evolution of Docker
The thought process of Docker is to provide the consistent outcome of application post deployment along with optimal use of > the resources. Before Docker it was Virtual Machines using Hypvervisor virtualization technique to make use of optimum physical server use.

> Physical server => Virtualization (Hypervisor) => Containerization (Docker) 

# Architectire of Docker
Docker which is containerization platform follows the client server aerchitecture where the docker commands are nothing but the client and server who is acting on these commands is deamon.

The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers.

The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface. 

*Reference:* [Docker Architecture](https://docs.docker.com/get-started/docker-overview/#docker-architecture)

# Docker Commands
If yoy're creating or containerizing the application you might need to run the below commands to spin up your application packaged as image.
- docker run
- docker run -i -t : -i means interactive and -t means attached to terminal.
- docker build
- docker build -t DOCKER_USERNAME/docker-image-name
- docker image ls : list down the images on current terminal
- docker push
- docker push <DOCKER_USERNAME>/docker-image-name
- 
# Docker networking
Docker creates a network interface to connect the container to the default network unless you specify the network explicitly this includes assigning an IP address to the container. Bydefault containers can connect to external networks using the host machine's network connection.

#Alternatives to Docker
