# Internship Sheet 5 - Reverse Proxy
A notice:
In the following tasks, you create services on your server serv-ws22that can be reached from
 the outside via a precisely defined IPv6 address from your IPv6 subnet. Please keep these 
 services running on your server at all times .

You can use the taskchecker [(LEA: https://lea.hochschule-bonn-rhein-sieg.de/goto.php?target=webr_1305668&client_id=db_040811)](http://[2001:638:408:200:ff00:cafe:0:beaf]:8080/) to check that your services are actually running .

The ongoing services are part of the pre- examination service (H-BRS) or the ULP (TH-Cologne).

 

In this task sheet, we will create an IPv6 reverse proxy for on-premises IPv6 services.

If you have not already done so, give your URL to a CDN provider for administration, who 
will offer access via https as an additional CDN service if required. (One possible CDN 
provider with a free basic plan is Cloudflare ).

 

# Task 1 - Nginx
In this task, we first create a "normal" web server, which we then expand into a reverse proxy in 
task 3. For DNS resolution in this task we will use "pure" DNS without CDN proxy capabilities.

a. Your web server should have the following IPv6 address from your IPv6 network 
my_ipv6 : 2001:638:408:200:ff??:cafe::7777/64

b. Create a Docker compose file (along with the necessary configuration files) for 
an nginx server reverse-proxythat has the following properties:
The server delivers a responsive website created with bootstrap and containing at least one image.
(Please don't just copy my solution, make something nice with bootstrap yourself.)
The server has the global IPv6 address specified above.
In addition to its external connection, the server is also connected to the local IPv6 network loc_ipv6 
so that it can easily be expanded into a reverse proxy. loc_ipv6 Task 3 provides services in the local 
IPv6 network that can be reached via the reverse proxy.
The server can be reached from outside via your URL www.yourDomain.xy. Set the URL address resolution to 
DNS with your CDN provider so that your URL resolves www.yourDomain.xyto . 2001:638:408:200:ff??:cafe::7777/64
If the server is called directly via its IPv6 address 2001:638:408:200:ff??:cafe::7777/64, an error message appears 
"Error 444 - Undefined Server Name / Access only allowed via www.yourDomain.xy"
Question : Why is it useful in many cases to block access to the web server using an IP address?

c. Provide your docker compose file and configuration file /etc/nginx/nginx.conf. Explain both files in detail.

d. Start and test your server.

My solution:

http://www.servmgmt.de
http://[2001:638:408:200:ff00:cafe:0:7777]
 

# Task 2 - HTTPS server
a. Enable CDN proxying for your server with the URL www.yourDomain.xy so that your server is reachable 
with http. Which IPv6 address is used to www.yourDomain.xyreach your server from the outside.

b. Set up the CDN proxy to support access to your Nginx server with both http(= unsecured) and with https.

c. How do you rate the security of your httpsweb server?
My solution:

http://www.servmgmt.de (access with http via CDN proxy function)
https://www.servmgmt.de (access with https via CDN proxy function)
 

# Task 3 - reverse proxy
a. Create a Docker compose file that  starts loc_ipv6 two miniwhoami services on the network 
miniwhoami-loc1and miniwhoami-loc2. Now set up your web server as a reverse proxy for these 
two miniwhoami services. Identify the configuration file you are using /etc/nginx/nginx.conf and 
explain this file in detail.

b. Set up two URLs miniwhoami-loc1.yourDomain.xyand miniwhoami-loc2.yourDomain.xypointing to your 
reverse proxy. Test access to the services miniwhoami-loc1 and miniwhoami-loc2.

c. An http echo service is a web server that, when called by a web browser, emits the http headers 
of the call. Find a suitable image for an http echo service echo.

d. Run the http echo service echo on your server serv-ws22behind your reverse proxy, so you echo can 
invoke the service through the reverse proxy in the following way:

http://echo.yourDomain.xyor.

https://echo.yourDomain.xy.

Compare and analyze the different calls.

My solution :

http://miniwhoami-loc1.servmgmt.de ,  http://miniwhoami-loc2.servmgmt.de

http://echo.servmgmt.de/ ,  https://echo.servmgmt.de/
 

# Task 4 - HTTPS redirection
a. Set up a server with the URL www2.yourDomain.xy . Enable the CDN proxy features so that your server is 
reachable with both http(unsecured) and https .

b. Adjust your configuration nginx.confso that a user access httpis www2.yourDomain.xy always httpsredirected to.
How do you proceed? Specify your nginx.conffile. Test access to your server www2.yourDomain.xy.
Note : With the CDN provider Cloudflare, you can set that yourDomain.xy every http access for all servers in your 
domain is automatically httpsredirected to . Please do not activate (or deactivate) this setting, otherwise the task 
makes little sense.

c. How do you rate the security of your web server www2.yourDomain.xy?

d. Analyze the TLS security of the access for your web server www2.yourDomain.xy  https with a suitable analysis tool.

Questions:
Which tool did you use?

What result did you get? What TLS versions are supported? Which TLS versions are not supported?

My solution for part b) of the task:

http://www2.servmgmt.de