# Internship Sheet 2 - Docker + Miniwhoami (local desktop)
 

### NOTE: where a naming convention was not specified throughout this project, I added my user name "migbin2s/" as a convenient naming convention for my docker images.

## Task 1 - Install Docker and Docker-Compose:
  `apt install docker`
  `apt install docker-compose`

### Which docker versions (client, server, API) have you installed?

* docker version
```
## Client:

 Version:           20.10.12

 API version:       1.41

 Go version:        go1.16.2

 Git commit:        20.10.12-0ubuntu2~20.04.1

 Built:             Wed Apr  6 02:14:38 2022

 OS/Arch:           linux/amd64
 
 Context:           default

 Experimental:      true

## Server:
    Engine:

  Version:          20.10.12

  API version:      1.41 (minimum version 1.12)

  Go version:       go1.16.2

  Git commit:       20.10.12-0ubuntu2~20.04.1

  Built:            Thu Feb 10 15:03:35 2022

  OS/Arch:          linux/amd64

  Experimental:     false

    containerd:
      Version:          1.5.9-0ubuntu1~20.04.4
      GitCommit:      

    runc:
       Version:          1.1.0-0ubuntu1~20.04.1
       GitCommit:        
    docker-init:
       Version:          0.19.0
```

* docker-compose version
```
docker-compose version 1.25.0, build unknown

docker-py version: 4.1.0

CPython version: 3.8.10

OpenSSL version: OpenSSL 1.1.1f  31 Mar 2020

```

## Task 2 - Primitive web server:
### (2b) (Test without Docker by just running the program). How do you proceed?
`export FLASK_APP=miniserver.py`
`flask run`

### (2c) For your program, create a Dockerfile servmgmt-ws22/prak/pr02/miniserver .Provide Dockerfile and explain the one for your web server.

Explanation of the Dockerfile:

```
#initialize a base image

* FROM python:3.8.10-alpine

#define the present working directory
* WORKDIR /miniserver

#copy the contents into the working directory
* ADD . /miniserver
  
#run pip to install the dependencies of the flask app
* RUN pip install -r requirements.txt
  
#define the command to start the container
* CMD ["python","miniserver.py"]

```
### (2d) Dockerfile - Create a Dockerimage from your miniserver with the tag v1. Which command do you use for this?
`docker build -t migbin2s/miniserver:v1 .`

### (2e) Run docker run your web server on your desktop system under the name miniserver_20221 as a Docker container on port 20221 . What is your docker-run command?

`docker run -p 20221:5000 --name miniserver_20221 migbin2s/miniserver:v1`


## Task 3 - Service miniwhoami:
### (3c) Create one for your program Dockerfile and build a Dockerimage with it miniwhoami. Provide your Dockerfile and the Docker command to create the image.
`docker build -t mgbin2s/miniwhoami:latest .`

### (3d) Deploy with docker run two containers miniwhoami_1 and miniwhoami_2 your web application miniwhoami on your Linux desktop system. To do this, use the ports 20231 and 20232. What are your docker-run commands?
`docker run -p 20231:80 --name miniwhoami_1 migbin2s/miniwhoami:latest`
`docker run -p 20232:80 --name miniwhoami_2 migbin2s/miniwhoami:latest`

## Task 4 - Network Troubleshooting
### (4a.i) Go docker exec into the container with miniwhoami_1.   What command do you use to do this?
`docker exec -it miniwhoami_1 sh`

### (4a.ii) Which of the following commands can you execute in the container: ping, ip a, curl, dig, nslookup ?
* ip a
* ping
* nslookup

### (4b.i) Now go inside the container miniwhoami_1 using the excellent Netzerk docker tool [nicolaka/netshoot](https://github.com/nicolaka). What command do you use to do this?
`docker run -it --net container:miniwhoami_1 nicolaka/netshoot`

### (4b.ii) Which of the following commands can you execute in the container: ping, ip a, curl, dig, nslookup ?
`I was able to execute all the listed commands above in the container`

### (4c) Can you access the container miniwhoami_1 from the container miniwhoami_2?
* Yes. I was able to ping miniwhoami_1 from miniwhoami_2 and vice versa

### (4d) How are the two containers miniwhoami_1 and miniwhoami_2 connected to each other?
* They belong to the same bridge network. Docker creates a default bridge network for all containers 
  created in the same host. And the containers can communicate with one another using their assigned IP addresses


### (4e) Can you access  heise.de  the website from the container miniwhoami_1?
* Yes. I was able to ping heise.de from miniwhoami_1

### (4f) mini_whoami_1 Specify and interpret the routing table in the container .
`ip route`
* default via 172.17.0.1 dev eth0  172.17.0.0/16 dev eth0 proto kernel scope link src 172.17.0.2 

-Container's IP address is: 172.17.0.2

-Default gateway is : 172.17.0.1

-Network ID: 172.17.0.0/16  









