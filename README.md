# Office 365 Addins on Docker

This example shows how to run office 365 addins as microservices on Docker containers 

[![Office365 Addin on Docker](https://github.com/spbreed/O365OnDocker/blob/master/readme-imgs/docker.png)](https://www.youtube.com/watch?v=PFVivUpMyLk)

<iframe width="560" height="315" src="https://www.youtube.com/embed/PFVivUpMyLk" frameborder="0" allowfullscreen></iframe>

Below sections would be covered in this sample

1. Why Docker?
2. Prerequisites
3. Configure Docker
4. Docker Components
5. Docker Commands
6. Build NodeJS Base Image
7. Build Office 365 NodeJS Microservice
8. Dockerize Office365 Microservice
9. Publish Microservice to Azure Docker Container
10. How to run this sample

![Micro services Architecture](https://github.com/spbreed/O365OnDocker/blob/master/readme-imgs/DockerArch.png)

## 1. Why Docker?
* Docker is an exciting new technology to build platform agnostic light weight apps/micro services/containers which can be provisioned, deployed, scaled faster than traditional VM's.
* Building Cloud hosted/provider hosted SharePoint/Office365 addins requires additional infrastructure to the mix. 
* Docker's X-Plat CLI tools to provide continous integration and delivery thus making DevOps relatively simpler.
For more details read [docker docs](https://docs.docker.com/)


## 2. Prerequisites
1. Windows 7 or Above
2. Office 365 subscription
3. Azure subscription
4. Visual Studio Code


## 3. Configure Docker
1. Install docker tool kit from [docker docs](https://docs.docker.com/engine/installation/windows/)
2. Run docker quick start terminal
<<<<<<< HEAD
(To run this sample straightaway skip to "How to run this sample" section)
=======
3. To run this sample straightaway skip to "[10. How to run this sample](https://github.com/spbreed/O365OnDocker/blob/master/README.md#10-how-to-run-this-sample)" section
>>>>>>> 94ae11d4e7532db729583f1d762c61ea7b0bed63


## 4. Docker Components
1. Docker Images > Blueprints of our application
2. Docker Container > Created from docker images and are real instances of our application
3. Docker Daemon > Building, running and distributing Docker containers
4. Docker Client > Run on our local machine and connect to the daemon
5. Docker Hub > A registry of docker images

## 5. Docker Commands
1. $docker build
2. $docker run
3. $docker search
4. $docker ps
5. $docker images
6. $docker rmi
7. $docker pull
8. $docker push
9. $docker-machine env
10. $docker-machine create
11. $docker attach

## 6. Build NodeJS Base Image
```
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y nodejs nodejs-legacy npm
RUN apt-get clean
```

## 7. Build Office 365 NodeJS Microservice
Run the sample Office 365 Add in from your local machine following [this article](https://github.com/OfficeDev/O365-Nodejs-Microsoft-Graph-Connect#configure-and-run-the-app)


## 8. Dockerize Office365 addin

* First install the required packages from package.json
```
COPY ./package.json src/
RUN cd src && npm install
```

* Then copy all the source code
```
COPY . /src
```

* Set working directory for docker daemon
```
WORKDIR src/
```

* Set default comment on start
```
CMD ["npm","start"]
```
* Now build the Docker Image and Run it

```
docker build -t o365addin-docker:0.1 .
docker run -d -p 80:3000 o365addin-docker:0.1
```

* Change the AppID and Redirection URL's in Azure and [authHelper.js](./authHelper.js)

* Test the app in browser 

## 9. Publish Microservice to Azure
1) Create SSL certs using Open SSL
```
$ openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout o365-docker.pem -out o365-docker.pem
$ openssl x509 -inform pem -in o365-docker.pem -outform der -out o365-docker.cer
$ openssl pkcs12 -export -out o365-docker.pfx -in o365-docker.pem -name "o365-docker Certificate"
```
2) Upload o365-docker.pem to Azure -> Settings -> Management Certificates -> Upload

3) Now Create a docker Microservice in Azure
```
docker-machine create -d azure --azure-subscription-id="d48ccdca-d4ab-4579-89fa-ed113033bf74" --azure-subscription-ce
rt="o365-docker.pem" --azure-location="East US" o365ondocker
```
4) Connect to Azure Docker machine

```
eval "$(C:/Program\ Files/Docker\ Toolbox/docker-machine.exe env o365ondocker)"
```

5) Change the AppID and Redirection URL's in Azure and [authHelper.js](./authHelper.js)

6) Build and Run the app
```
docker build -t o365addin-docker:0.1 .
docker run -d -p 80:3000 o365addin-docker:0.1
```
7) Test the app in browser

<<<<<<< HEAD
##How to run this sample
=======
## 10. How to run this sample
>>>>>>> 94ae11d4e7532db729583f1d762c61ea7b0bed63

1) Open Docker quick start terminal
2) Clone this repo
```
git clone https://github.com/spbreed/O365OnDocker.git
```
3) Update Office 365 App permissions and authhelper.js with Docker IP
```
docker-machine ip default
```
4) Build the docker image
```
<<<<<<< HEAD
$ docker build -t o365addin-docker:0.1 .
=======
docker build -t o365addin-docker:0.1 .
>>>>>>> 94ae11d4e7532db729583f1d762c61ea7b0bed63

```
5) Run the docker container
```
<<<<<<< HEAD
$ docker run -d -p 80:3000 o365addin-docker:0.1
=======
docker run -d -p 80:3000 o365addin-docker:0.1
>>>>>>> 94ae11d4e7532db729583f1d762c61ea7b0bed63
```

