# Internship Sheet 2 - Docker + Miniwhoami (local desktop)
 

# Task 1 - Install Docker and Docker-Compose
a.Install the latest version of Docker on your Linux desktop system. What versions (client, server, API) do you have installed?

b.Install on your Linux desktop system docker composein a current version. What command did you docker compose use to install? Which version do you have installed?
 

# Task 2 - Primitive web server
a.In a subdirectory, create servmgmt-ws22/prak/pr02/miniserver a program for a small, very simple web server miniserverthat responds with "Hello Service Management winter term 2022. My name is ....".
Requirement : The web server should be programmed in a programming language (eg PHP, Node.js, Python, Go) so that the server can be functionally expanded.

b.Test your web server - i.e. the program you have created - on your Linux desktop system. (Test without Docker by just running the program). How do you proceed?

c.For your program, create servmgmt-ws22/prak/pr02/miniserver a Dockerfile.
Provide Dockerfileand explain the one for your web server.

d.DockerfileCreate a Dockerimage from your miniserverwith the tag v1. Which command do you use for this?
Run docker run your web server on your desktop system under the name miniserver_20221 as a Docker container on port 20221 . What is your docker-run command?
The port number 20221is structured as follows:

2 stands for the course Services Management in Networks,

02 for internship sheet 2,

2 for task 2 on this internship sheet,

1 is the number of the container in the task.
 

# Task 3 - Service miniwhoami
a.Create a subdirectory servmgmt-ws22/prak/pr02/miniserver. In this you expand your web server program miniserverto a web service miniwhoamiwith the following properties:
The web service miniwhoamidisplays the host name, the IP addresses (IPv4 and IPv6) in the runtime environment and the number of accesses.
The background color of miniwhoamichanges depending on the hostname.
Specifically : A different hostname results in a different background color. This property is useful, for example, for the analysis of load balancing scenarios, because it allows you to see immediately whether miniwhoamithe host name has changed when accessing the web service, i.e. another container has responded.

b.First test your application without Docker on your local machine.

c.Create one for your program Dockerfileand build a Dockerimage with it miniwhoami. Provide your Dockerfile and the Docker command to create the image.

d.Deploy with docker run two containers miniwhoami_1 and miniwhoami_2 your web application miniwhoami on your Linux desktop system. To do this, use the ports 20231and 20232. What are your docker-run commands?

e.Call both services in your browser and test the service you have programmed miniwhoami.
 

# Task 4 - Network Troubleshooting
a.Go docker exec into the container with miniwhoami_1.
What command do you use to do this?
Which of the following commands can you execute in the container: ping, ip a, curl, dig, nslookup ?

b.Now go inside the container miniwhoami_1 using the excellent Netzerk docker tool [nicolaka/netshoot](https://github.com/nicolaka).
What command do you use to do this?
Which of the following commands can you execute in the container: ping, ip a, curl, dig, nslookup ?

c.Can you access the container mini_whoami_1 from the container mini_whoami_2?

d.How are the two containers mini_whoami_1 and mini_whoami_2connected to each other?

e.Can you access mini_whoami_1the website from the container heise.de?

f.Specify and interpret the routing table in the container mini_whoami_1.