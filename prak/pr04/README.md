# Internship Sheet 4 - IPv6

## Task 1 - remote deployment of miniwhoami

### (1b) Create miniwhoami a tagged Docker registry for your web application Dockerimage . Which docker command can you use for this?

`docker login docker.fslab.de`

`docker tag migbin2s/miniwhoami:latest docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami:latest`

### (1c) Use the app miniwhoami to push your image up to the registry at https://git.fslab.de .

`docker push docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami:latest`

### (1d) Deploy docker run a container of miniwhoami_20411 your web application miniwhoami to your lab server serv-ws22. Use the port for this 20411. Which command do you use for this?

`docker pull docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami`
  
`docker run -p 20411:80 --name miniwhoami_20411 --restart=always -d docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami`

### (1e)  Deploy two services miniwhoami_20412 and miniwhoami_20413 your web application miniwhoami to your lab server using a docker-compose file serv-ws22. To do this, use the ports 20412 and 20413. Both containers should be connected to mynetworkeach other via a common network.
#
Find the docker-compose file in the folder pr04

`docker compose up -d`
#
My solution:

http://[2001:638:408:200:ff6c::1]:20411

http://[2001:638:408:200:ff6c::1]:20412

http://[2001:638:408:200:ff6c::1]:20413


## Task 2 - Investigation of network structures
### (2a) Your three miniwhoami containers miniwhoami_20411, miniwhoami_20412 and miniwhoami_20413 each have an IPv4 interface eth0. Create a list for your three miniwhoamicontainers with the columns: container name, IP address, netmask, default gateway. Does the interface eth0 also have an IPv6 address?
#
The interface eth0 for the containers do not have IPv6 addresses
#
| ContainerName     | IP_Address        | NetMask            |DefaultGateway      |	
|:-----------------:|:-----------------:|:------------------:|:------------------:|
|miniwhoami_20411   |172.17.0.2         | 255.255.0.0	       |172.17.0.1          |
|miniwhoami_20412   |172.19.0.2         | 255.255.0.0        |172.19.0.1          |
|miniwhoami_20413   |172.19.0.3         | 255.255.0.0        |172.19.0.1          |

### (2b) docker network ls Output a list of docker nets with the command . Which networks are your containers connected to? What command are you using to determine this?

`docker ps --format "{{.Names}} {{.Networks}}"`
#
```
 miniwhoami_20412 mynetwork
 miniwhoami_20413 mynetwork
 miniwhoami_20411 bridge

 ```

### (2c) Go into your miniwhoami containers and test which containers give you access to the outside world. Interpret your result.
The three containers gave me access to the outside world.

### (2d) Why can you reach the containers with IPv6 from the outside at all?
 A docker container is assigned an IPv4 address in a private range which the Docker daemon will then NAT to the host's address. Hence, we can do port forwarding from containers to be visible at ports on the host's IP.

### (2e) Use the tool to go netshoot into the mesh where the container miniwhoami_20411 is located. Can the container be pinged using the IP address and the service name?

`docker run --rm -it --network bridge  nicolaka/netshoot`

I was able to ping the container using the IP address but pinging was not possible with the name miniwhoami_20411 . This is because container names are not resolved in the docker default bridge network, they only communicate using IP address.

### (2f) Use the tool to go netshoot into the mesh mynetwork where the containers miniwhoami_20412 and miniwhoami_20413 are located. Can the two containers be pinged using the IP address and the service name?

`docker run --rm -it --network mynetwork  nicolaka/netshoot`

Yes, I was able to ping both containers using the IP address and the service name?

### (2g) Test dig the Docker-internal DNS resolution of the services miniwhoami_20412 and miniwhoami_20413. What is your concrete approach and what is the result?

`docker run --rm -it --network mynetwork nicolaka/netshoot`

`dig miniwhoami_20412`

>; <<>> DiG 9.18.3 <<>> miniwhoami_20412
>;; global options: +cmd
>;; Got answer:
>;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28384
>;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
>
>;; QUESTION SECTION:
>;miniwhoami_20412.              IN      A
>
>;; ANSWER SECTION:
>miniwhoami_20412.       600     IN      A       172.19.0.2
>
>;; Query time: 0 msec
>;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
>;; WHEN: Sat Oct 29 19:42:20 UTC 2022
>;; MSG SIZE  rcvd: 66

`dig miniwhoami_20413`

>; <<>> DiG 9.18.3 <<>> miniwhoami_20413
>;; global options: +cmd
>;; Got answer:
>;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 20495
>;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
>
>;; QUESTION SECTION:
>;miniwhoami_20413.              IN      A
>
>;; ANSWER SECTION:
>miniwhoami_20413.       600     IN      A       172.19.0.3
>
>;; Query time: 0 msec
>;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
>;; WHEN: Sat Oct 29 19:47:46 UTC 2022
>;; MSG SIZE  rcvd: 66

## Docker+IPv6
##  We will now activate IPv6 in Docker and configure it so that containers can be operated with their own IPv6 addresses.

## Task 3 - Docker IPv6 activation
### (3a.i) Enable IPv6 on your server for your Docker installation by creating the file /etc/docker/daemon.json and configuring it appropriately. In the variable "fixed-cidr-v6", the Docker daemon should be given a suitable /80 subnet to use for managing Docker containers. Take this IPv6 subnet from the IPv6 subnet assigned to you /78 . To do this, divide the address space assigned to you into four smaller /80-subnets. Specify these subnets (which you own) explicitly in the solution to this task. From these four subnets, select a subnet that does not contain the address of your server. You pass this subnet to the Docker daemon via the variable "fixed-cidr-v6"to administration. 
#
Address space assigned to me: 2001:638:408:200:FF6C::/78 

IPv6 address of my server: 2001:638:408:200:FF6C::1/64
#
I divided the address space assigned to me into these four smaller /80-subnets :
* 2001:638:408:200:FF6C::/80
* 2001:638:408:200:FF6D::/80   -------> I gave this subnet to the Docker daemon to use for managing Docker containers
* 2001:638:408:200:FF6E::/80
* 2001:638:408:200:FF6F::/80
#

`nano /etc/docker/daemon.json`
```

{
  "ipv6": true,
  "fixed-cidr-v6": "2001:638:408:200:ff6d::/80"
}

```

`systemctl restart docker`

### (3a.ii) Which bridge is assigned to the subnet you have chosen?
The chosen subnet 2001:638:408:200:FF6D::/80 is assigned to the default docker bridge network, docker 0.

### (3b.i) Which IPv6 address is displayed when you call up miniwhoami_20411 in the browser? Which interface has this IPv6 address. How is this address formed?

 * 2001:638:408:200:ff6d:242:ac11:2
  
This IPv6 address belongs to the IPv6 interface of the container miniwhoami_20411
The IPv6 address was formed using a combination of 80bits we provided in the IPv6 prefix for the subnet and 48bits from the MAC address of the container.

### (3b.ii) From where can the container miniwhoami_20411 be pinged using this IPv6 address? Which IPv6 addresses can be miniwhoami_20411 reached via ping?

### (3b.iii) miniwhoami_20411 In the containing container , check ip -6 r the routing table. Is the /80 IPv6 subnet routed correctly? What is the routing problem, do you analyze the situation?

### (3b.iv) Why is the container miniwhoami_20411 reached via the IPv6 server address (nevertheless)?
By default, a docker container is assigned an IPv4 address in a private range which the Docker daemon will then NAT to the host's address. Hence, we can do port forwarding from containers to be visible at ports on the host's IP address

### (3b.v) What requirements must be met so that the container miniwhoami_20411 can be reached from outside via its own IPv6 address?
NDP proxy has to be enabled such that the Docker host can listen to neighbour solicitation requests for the container's IP address and send a confirmation response that it is responsible for the IP address.

## Task 4 - Docker IPv6 NDP

### (4a) Manual configuration of the NDP function: 
### First switch sysctl net.ipv6.conf.ens??.proxy_ndp=1 on the NDP proxy function for the correct interface. Use to configure ip -6 neigh add ...  the IPv6 address to which the NDP proxy function should apply. What is the command in detail in your case? Test the NDP functionality by pinging the  IPv6 address of the container miniwhoami_20421 from the outside  (see task 5b).

`sysctl net.ipv6.conf.ens18.proxy_ndp=1`

`ip -6 neigh add proxy 2001:638:408:200:ff6d:242:ac11:2 dev ens18`

### (4b) Installing an NDP Proxy: 
### The NDP Proxy Daemon ndppd(see `GIT`) is also provided via the Ubuntu repo. install with `apt install ndppd`. You have a template for the required configuration `/etc/ndppd.conf` file in the file `/usr/share/doc/ndppd/ndppd.conf-dist`. rule 1111::You only have to customize the interface and the rules ( ). Specifically, you need to 1111:: replace with your subnet (more precisely: replace with the subnet for which you want the NDP function). If you want to NDP more than one subnet, specify multiple rules. The autostart of ndppd was already activated for me; otherwise activate it via systemctl. Identify the configuration file you are using ndppd.conf and explain the most important elements of it.

````
route-ttl 30000
proxy ens18 {
   router true
   timeout 500
   ttl 30000

   rule 2001:638:408:200:FF6D::/80 {

      static
   }
}

````
`systemctl restart ndppd`

* The  configuration file must contain one or more proxy sections, and each of these section must contain one or more rule sections.
* The ndppd daemon listens on the interface specified . Once  a  Neighbor  Solicitation  message  arrives, it will try to match the target address against the address specified as the argument of the rule section.
* proxy ens18 : Adds the proxy and binds it to the ens18 interface.  
* ttl 30000 : sets  ndppd to cache  an entry for 30000milliseconds.
*  static : sets ndppd to immediately respond to a  Neighbor Solicitation  Message without querying an  internal interface.
*  router <true/false> : Controls if ndppd should send the router bit when  sending Neighbor Advertisement messages
  
## Task 5 - Public IPv6 Subnet
### A public IPv6 Docker subnet my_ipv6 should be set up in the address space 2001:638:408:200:ff??::/78  assigned to you . This subnet is used for dedicated public IPv6 servers with static IPv6 addresses.
### The address space of this IPv6 subnet my_ipv6 should be specified as follows: 2001:638:408:200:ff??:cafe::/96. (?? = lowest possible address.)
#
### (5a) Is the size of this subnet sufficient? Justify your answer.
Yes, (128 - 96) = 32bits of address space is still available.

### (5b) Set up your IPv6 subnet  my_ipv6on your server serv-ws22. What is the mesh generation command?

` docker network create --subnet "2001:638:408:200:ff6c:cafe::/96" --ipv6 my_ipv6`

### (5c) Adjust the configuration file ndppd.conf so that the NDP Proxy Daemon also resolves ndppd addresses from the IPv6 subnet my_ipv6.

```
route-ttl 30000
proxy ens18 {
   router true
   timeout 500
   ttl 30000

   rule 2001:638:408:200:ff6d::/80 {

      static
   }

   rule 2001:638:408:200:ff6c:cafe::/96 {

      static
   }
}

```
`systemctl restart ndppd`

## Task 6 - Services with public IPv6 address
### You have the following three global IPv6 addresses in your IPv6 subnet my_ipv6  :
* ipv6_1: 2001:638:408:200:ff6c:cafe::1111/96
* ipv6_2: 2001:638:408:200:ff6c:cafe::2222/96
* ipv6_3: 2001:638:408:200:ff6c:cafe::3333/96
#
### (6a) Deploy docker run your web application miniwhoami to your lab server as a container with global IP address ipv6_1 serv-ws22. Which command do you use for this?
 `docker run --restart always -d --network my_ipv6 --ip6=2001:638:408:200:ff6c:cafe::1111 --name=miniwhoami1 docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami`

### (6b)  Deploy two more instances of your web application miniwhoami with global IP addresses ipv6_2 and ipv6_3 to your lab server serv-ws22 using a docker-compose file . Enter your compose file.

 ```
services:
  miniwhoami2:
    container_name: miniwhoami2
    image: docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami
    networks:
      my_ipv6:
        ipv6_address: 2001:638:408:200:ff6c:cafe::2222
    restart: always

  miniwhoami3:
    container_name: miniwhoami3
    image: docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami
    networks:
      my_ipv6:
        ipv6_address: 2001:638:408:200:ff6c:cafe::3333
    restart: always

networks:
  my_ipv6:
    name: my_ipv6
    external: true

```
`docker compose -f docker-compose_2.yml up -d`

### (6c) You now have three services miniwhoami1, miniwhoami2 and miniwhoami3 deployed, each of which can be reached via its own IPv6 address. Configure your domain in Cloudflare so that your services are reachable via miniwhoami1.yourDomain.xy, ,  miniwhoami2.yourDomain.xy and  miniwhoami3.yourDomain.xy.

* My Solution:
  
http://[2001:638:408:200:ff6c:cafe::1111] ,   http://miniwhoami1.migbin2s-servemgmt.site

http://[2001:638:408:200:ff6c:cafe::2222] ,   http://miniwhoami2.migbin2s-servemgmt.site

http://[2001:638:408:200:ff6c:cafe::3333] ,   http://miniwhoami3.migbin2s-servemgmt.site

### (6d) Ping my service miniwhoami1 using the url miniwhoami1.servmgmt.de. What did you notice? How do you interpret the result?
As seen below the ICMP echo reply is not coming from your configured IP address `2001:638:408:200:ff00:cafe::1111` . It is coming from `2a06:98c1:3120:::` which is a Cloudflare IP address due to Cloudflare proxying. The ping did not go through the proxy because Ping uses the ICMP protocol to communicate, but Proxy only supports TCP and UDP packets.
```
Pinging miniwhoami1.servmgmt.de [2a06:98c1:3120::] with 32 bytes of data:
Reply from 2a06:98c1:3120::: time=14ms
Reply from 2a06:98c1:3120::: time=12ms
Reply from 2a06:98c1:3120::: time=10ms
Reply from 2a06:98c1:3120::: time=8ms

Ping statistics for 2a06:98c1:3120:::
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 8ms, Maximum = 14ms, Average = 11ms

```

#

## Task 7 - Local IPv6 subnet
### (7a) Create in serv-ws22 a local IPv6 network loc_ipv6 with the address space fd00:dead:beef::/48 on your Docker host . What command do you use to do this? loc_ipv6 Our local IPv6 services will run on the network .

`docker network create --subnet "d00:dead:beef::/48" --ipv6 loc_ipv6`



 