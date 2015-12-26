# Office 365 Addins on Docker

Docker is an exciting new technology to build cloud agnostic apps which runs in any platform.  
This example shows how to run office 365 addins using Docker containers 

Below sections would be covered in this sample

> [Prerequisites](#Prerequisites)
2. [Why Docker?](#Why Docker?)
3. [Configure Docker](#Configure Docker)
4. [Build NodeJS Base Image](#Build NodeJS Base Image)
5. [Configure Office 365 NodeJS addin](#Configure Office 365 NodeJS addin)
6. [Dockerize Office365 addin](#Dockerize Office365 addin)
7. [Run Docker from Azure](#Run Docker from Azure)

##Prerequisites
1. Windows 7 or Above
2. Office 365 subscription
3. Azure subscription

#Why Docker?
Docker provides tools and technology to build platform agnostic apps/micro services/containers which can be provisioned, deployed, scaled faster than traditional VM's.
Building Cloud hosted/provider hosted SharePoint/Office365 addins requires additional infrastructure to the mix. Docker makes it easy to deploy and maintain through X platform cli interface.
For more details read [docker docs](https://docs.docker.com/)


##Configure Docker
1. Install docker tool kit from [docker docs](https://docs.docker.com/engine/installation/windows/)
2. Run docker quick start terminal

##Configure Office 365 NodeJS addin
Run the sample Office 365 Add in from your local machine following [this article](https://github.com/OfficeDev/O365-Nodejs-Microsoft-Graph-Connect#configure-and-run-the-app)

##Build NodeJS Base Image

* Prepare Docker file
Refer [Docker file](./DockerFile) 

```
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y nodejs nodejs-legacy npm
RUN apt-get clean
```

##Dockerize Office365 addin

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
$ docker build -t o365addin-docker:0.1 .
$ docker run -d -p 80:3000 o365addin-docker:0.1
```

* Change the AppID and Redirection URL's in Azure and [authHelper.js](./authHelper.js)

* Test the app in browser 

##Run Docker from Azure
1) Create SSL certs using Open SSL
```
$ openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout o365-docker.pem -out o365-docker.pem
$ openssl x509 -inform pem -in o365-docker.pem -outform der -out o365-docker.cer
$ openssl pkcs12 -export -out o365-docker.pfx -in o365-docker.pem -name "o365-docker Certificate"
```
2) Upload o365-docker.pem to Azure -> Settings -> Management Certificates -> Upload

3) Now Create a docker Microservice in Azure
```
$ docker-machine create -d azure --azure-subscription-id="d48ccdca-d4ab-4579-89fa-ed113033bf74" --azure-subscription-ce
rt="o365-docker.pem" --azure-location="East US" o365ondocker
```
4) Connect to Azure Docker machine

```
$ eval "$(C:/Program\ Files/Docker\ Toolbox/docker-machine.exe env o365ondocker)"
```

5) Change the AppID and Redirection URL's in Azure and [authHelper.js](./authHelper.js)

6) Build and Run the app
```
$ docker build -t o365addin-docker:0.1 .
$ docker run -d -p 80:3000 o365addin-docker:0.1
```
7) Test the app in browser
