# What is Docker?

An open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation of OS-level virtualization on Linux.

Source:Wikipedia

# What are containers?

The industry standard today is to use Virtual Machines (VMs) to run software applications. VMs run applications inside a guest Operating System, which runs on virtual hardware powered by the server’s host OS.

VMs are great at providing full process isolation for applications: there are very few ways a problem in the host operating system can affect the software running in the guest operating system, and vice-versa. But this isolation comes at great cost — the computational overhead spent virtualizing hardware for a guest OS to use is substantial.

Containers take a different approach: by leveraging the low-level mechanics of the host operating system, containers provide most of the isolation of virtual machines at a fraction of the computing power.

Source: https://docker-curriculum.com/

#Docker quick start?

##Pre-requisite
	- 1) Install docker in windows
	
		Docker Destop installed
		
		> docker --version

	- 2) Check docker deamon is running

		> docker version command
 
			Gives information dcoker client and server
			Docker deamon(server) is running or not
			Gives docker version and other details
			
	- 3) Basic commands to start with docker
	
		> docker info 
			
			Display system-wide information
			
		> docker --help
			
			Get help with Docker. Can also use –help on all subcommands	
			
			> docker images --help
			> docker ps --help
			> docker container ls --help
			
	- 4) Understand absics

		Image : Docker image is a template for running container. You can either create your own docker image or download existing images from docker hub
		
		Run : Once you have docker image, now you can run it.
		
		Container: Once you run a docker image, it create a container which is running instance of image
					
	
	## 3) Run a docker container
	
		###a) Hello world : Simple container which exit after printing Hello World
		
			> docker run hello-world
		
		###b) Busybox : BusyBox is a software suite that provides several Unix utilities in a single executable file.
					 "The Swiss Army knife of Embedded Linux"
		
			> docker run busybox
			
			> docker run busybox echo "hello from busybox"
			
			> docker run -it busybox : Now busybox shell is opened and you can run multiple commands there.
			
				- ls
				- uptime
				- pwd
				
			
		###c) First web server app and check using browser
		
			> docker run -d -p 8080:80 docker/welcome-to-docker
			
			http://localhost:8080/
			
		###d) Redis database
		
			Redis Server
		
			> docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
			
			-d : deattached
			
			--name : name of container
			
			-p : port <host_machine_port>:<container_port>
			
			Redis Server + Redis Insight 
			(Redis Insight is our free graphical interface for analyzing Redis data across all operating systems)
			
			> docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
			
			Open http://localhost:80001
			
			Connecting to Redis CLI
			
			> docker exec -it redis-stack redis-cli
			
				127.0.0.1:6379> set name Deep
				OK
				127.0.0.1:6379> get name
				"deep"0
				
		e)
			
		
		docker image rm <image_name>
		
		docker container rm <container_id>
		
		docker container prune : to remove all stopped container
		
		> docker container ls or 
		> docker ps
			
			List container
			
				-a : Show all container, running and stopped
			
		> docker images
		
			List images
			
				docker images | find "hello-world"
				
				
	#4) Create custom docker image
	
	##a) Linex hello world
	
Dockerfile
FROM ubuntu
MAINTAINER test-user
RUN apt update
CMD ["echo", "Hello World"]

> docker build -t ubuntu-hello-img .

> docker run --name test-ubuntu-hello ubuntu-hello-img 

Source: https://phoenixnap.com/kb/create-docker-images-with-dockerfile

##b) 
Dockerfile
FROM nginx:latest 
COPY source/html /usr/share/nginx/html 
# expose port, port 80 is used by default by nginx
#EXPOSE 80, we can comment it out
CMD ["nginx", "-g","daemon off;"]


###c) Spring boot application
Dockerfile
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]

docker build -t springio/gs-spring-boot-docker .

Source : https://spring.io/guides/gs/spring-boot-docker
	
	###b) Python application
	
Dockerfile
	
FROM python:3.8-slim
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]

> docker build -t <image_name> .

	docker build: Instructs Docker to build an image.

	-t <image_name>: Assigns a name and optional tag to the image.

	.: Indicates that the Dockerfile is in the current directory. 
	
	> docker build -t my_python_app

> docker run -p <host_port>:<container_port> <image_name>

	docker run: Starts a container from the specified image
	
	-p <host_port>:<container_port>: Maps a port from the host machine to a port inside the container 
	
	> docker run -p 8000:5000 my_python_app

> Open browser localhost:8000	
	
	4) Dealing with Docker Hub
	
		DOCKER HUB
		Docker Hub is a service provided by Docker for finding and sharing container images with your team. Learn more and find images at https://hub.docker.com
	
	
		> docker login -u <username>
		
			Login into Docker
			
		> docker push <username>/<image_name>
			
			Publish an image to Docker Hub

		> docker search <image_name>
		
			Search Hub for an image

		> docker pull <image_name>
		
			Pull an image from a Docker Hub



- Run you first docker containers


References:
https://docker-curriculum.com/
https://docs.docker.com/get-started/introduction/get-docker-desktop/
https://medium.com/redis-with-raphael-de-lio/how-to-run-redis-locally-in-a-docker-container-and-manage-it-with-redis-insight-and-redis-cli-14b0af54e1d2
https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/#:~:text=To%20get%20started%20with%20Redis,Insight%20to%20visualize%20your%20data.
