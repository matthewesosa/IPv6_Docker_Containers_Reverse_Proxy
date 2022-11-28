# Internship Sheet 4 - IPv6
A notice:
In the following tasks, you create services on your server serv-ws22that can be reached from the outside via a precisely defined IPv6 address from your IPv6 subnet. Please keep these services running on your server at all times .

You can use the taskchecker [(LEA: https://lea.hochschule-bonn-rhein-sieg.de/goto.php?target=webr_1305668&client_id=db_040811 )](http://[2001:638:408:200:ff00:cafe:0:beaf]:8080/) to check that your services are actually running .

The ongoing services are part of the pre- examination service (H-BRS) or the ULP (TH-Cologne).

 

# Task 1 - remote deployment of miniwhoami
In practice sheet 2, task 2, you miniwhoami created an image for your program locally on your Linux desktop system. This local image should now be pushed into a publicly accessible Docker registry. From there, the image can be conveniently docker runinstalled (deployed) on any Docker host using the command.

a. You already have a Docker Registry ( ) on your Gitlab account ( https://git.fslab.dedocker.fslab.de ). Familiarize yourself with the operation of this registry. In particular, test access to this registry. (Link to this: https://docs.gitlab.com/ee/user/packages/container_registry/ )

b. Create miniwhoami a tagged Docker registry for your web application Dockerimage . Which docker command can you use for this?

c. Use the app miniwhoamito push your image up to the registry at https://git.fslab.de .

d. Deploy docker runa container of miniwhoami_20411your web application miniwhoami to your lab server serv-ws22. Use the port for this 20411. Which command do you use for this?

e. Deploy two services miniwhoami_20412 and miniwhoami_20413 your web application miniwhoami to your lab server using a docker-compose file serv-ws22. To do this, use the ports 20412 and 20413. Both containers should be connected to mynetworkeach other via a common network.

How do you rate the effort involved in deployment? Could deployment be simplified even further?
My solution:

http://[2001:638:408:200:ff00::1]:20411
http://[2001:638:408:200:ff00::1]:20412
http://[2001:638:408:200:ff00::1]:20413

Note : After you enable IPv6 on serv-ws22 (see Task 3), you will also see your container's IPv6 address, if it has any.

 

# Task 2 - Investigation of network structures
a. Your three miniwhoamicontainers miniwhoami_20411, miniwhoami_20412and miniwhoami_20413 each have an IPv4 interface eth0. Create a list for your three miniwhoamicontainers with the columns: container name, IP address, netmask, default gateway. Does the interface eth0also have an IPv6 address?

b. Output a list of docker nets with the command docker network ls . Which networks are your containers connected to? What command are you using to determine this?

c. Go into your miniwhoamicontainers and test which containers give you access to the outside world. Interpret your result.

d. Why can you reach the containers with IPv6 from the outside at all?

e. Use the tool to go netshootinto the mesh where the container miniwhoami_20411is located. Can the container be pinged using the IP address and the service name?

f. Use the tool to go netshootinto the mesh mynetworkwhere the containers miniwhoami_20412and miniwhoami_20413are located. Can the two containers be pinged using the IP address and the service name?

g. Test dig the Docker-internal DNS resolution of the services miniwhoami_20412 and miniwhoami_20413.
    What is your concrete approach and what is the result?

 

# Docker+IPv6
At the beginning of the course, each of you received a public IPv6 address space for your own administration. For example, I have the public IPv6 address space for my use 2001:638:408:200:ff00::/78. serv-ws22So far I've only used the IPv6 address from this address space for my Docker host 2001:638:408:200:ff00::1. serv-ws22I would now like to operate Docker services with their own IPv6 addresses on my Docker host .


To do this, we will now activate IPv6 in Docker and configure it so that containers can be operated with their own IPv6 addresses.

 

# Task 3 - Docker IPv6 activation
a. Enable IPv6 on your server for your Docker installation by creating the file /etc/docker/daemon.jsonand configuring it appropriately. In the variable "fixed-cidr-v6", the Docker daemon should be given a suitable /80 subnet to use for managing Docker containers. Take this IPv6 subnet from the IPv6 subnet assigned to you /78 . To do this, divide the address space assigned to you into four smaller /80-subnets. Specify these subnets (which you own) explicitly in the solution to this task. From these four subnets, select a subnet that does not contain the address of your server. You pass this subnet to the Docker daemon via the variable "fixed-cidr-v6"to administration. Which bridge is assigned to the subnet you have chosen?

b. Access your container miniwhoami_20411 with a browser over IPv6 .

Questions:
Which IPv6 address is displayed when you call up miniwhoami_20411 in the browser? Which interface has this IPv6 address. How is this address formed?

From where can the container miniwhoami_20411 be pinged using this IPv6 address? Which IPv6 addresses can be miniwhoami_20411 reached via ping?

In the containing container miniwhoami_20411 , check ip -6 rthe routing table. Is the /80IPv6 subnet routed correctly? What is the routing problem, do you analyze the situation?

Why is the container miniwhoami_20411 reached via the IPv6 server address (nevertheless)?

What requirements must be met so that the container miniwhoami_20411can be reached from outside via its own IPv6 address?
 

# Task 4 - Docker IPv6 NDP
In order for your containers to be accessible over IPv6, the server's IPv6 interface must serv-ws22 act as an NDP proxy. This is the only way to resolve the IPv6 address of a container behind the IPv6 interface. The proxy thus handles the NDP protocol externally for the containers behind the IPv6 interface.

a. Manual configuration of the NDP function:
   First switch sysctl net.ipv6.conf.ens??.proxy_ndp=1on the NDP proxy function for the correct interface. Use to configure ip -6 neigh add ...  the IPv6 address to which the NDP proxy function should apply. What is the command in detail in your case? Test the NDP functionality by pinging the container's IPv6 address from the outside miniwhoami_20421(see task 5b).

b. Installing an NDP Proxy:
   The NDP Proxy Daemon ndppd(see GIT ) is also provided via the Ubuntu repo. install with apt install ndppd. You have a template for the required configuration /etc/ndppd.conf file in the file /usr/share/doc/ndppd/ndppd.conf-dist. rule 1111::You only have to customize the interface and the rules ( ). Specifically, you need to 1111:: replace with your subnet (more precisely: replace with the subnet for which you want the NDP function). If you want to NDP more than one subnet, specify multiple rules. The autostart of ndppd was already activated for me; otherwise activate it via systemctl.

c. Identify the configuration file you are using ndppd.conf and explain the most important elements of it.
 

# Task 5 - Public IPv6 Subnet
2001:638:408:200:ff??::/78A public IPv6 Docker subnet should be set up in the address space assigned to you my_ipv6. This subnet is used for dedicated public IPv6 servers with static IPv6 addresses.


The address space of this IPv6 subnet my_ipv6should be specified as follows: 2001:638:408:200:ff??:cafe::/96. (?? = lowest possible address.)


Example : My concrete address space for the IPv6 subnet my_ipv6 is: 2001:638:408:200:ff00:cafe::/96.

a. Is the size of this subnet sufficient? Justify your answer.

b. Set up your IPv6 subnet  my_ipv6 on your server serv-ws22. What is the mesh generation command?

c. Adjust the configuration file ndppd.conf so that the NDP Proxy Daemon also resolves ndppdaddresses from the IPv6 subnet . my_ipv6
 

# Task 6 - Services with public IPv6 address
You have the following three global IPv6 addresses in your IPv6 subnet my_ipv6  :

ipv6_1:  2001:638:408:200:ff??:cafe::1111/96

ipv6_2:2001:638:408:200:ff??:cafe::2222/96

ipv6_3:2001:638:408:200:ff??:cafe::3333/96

The characters ?? represent the two hex numbers of the public IPv6 address space assigned to you 2001:638:408:200:ff??::/78.

a. Deploy docker runyour web application miniwhoami to your lab server as a container with global IP address ipv6_1 serv-ws22. Which command do you use for this?

b. Deploy two more instances of your web application miniwhoami  with global IP addresses ipv6_2 and ipv6_3 to your lab server using a docker-compose file serv-ws22. Enter your compose file.

c. You now have three services miniwhoami1, miniwhoami2 and miniwhoami3deployed, each of which can be reached via its own IPv6 address. Configure your domain url.my in Cloudflare so that your services are reachable via miniwhoami1.yourDomain.xy, ,  miniwhoami2.yourDomain.xyand  miniwhoami3.yourDomain.xy.

d. Ping my service miniwhoami1using the url miniwhoami1.servmgmt.de. What did you notice? How do you interpret the result?

My solution:

http://[2001:638:408:200:ff00:cafe::1111]  ,   http://miniwhoami1.servmgmt.de
http://[2001:638:408:200:ff00:cafe::2222]  ,   http://miniwhoami2.servmgmt.de
http://[2001:638:408:200:ff00:cafe::3333]  ,   http://miniwhoami3.servmgmt.de
 

# Task 7 - Local IPv6 subnet
a.Create serv-ws22a local IPv6 network loc_ipv6 with the address space on your Docker host fd00:dead:beef::/48. What command do you use to do this? loc_ipv6 Our local IPv6 services will run on the network .

b.Explain the address fd00:dead:beef::/48 according to RFC4193 .

c.Go netshootinto the net with the tool loc_ipv6. Which IP addresses does the tool have netshootin the network loc_ipv6and which default routes are loc_ipv6 entered for the network?

d.Use to investigate pingwhich connections serv-ws22 are possible on your docker host between networks and containers with IPv4 and IPv6.