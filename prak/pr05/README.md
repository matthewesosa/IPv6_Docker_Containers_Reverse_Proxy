# In this task sheet, we will create an IPv6 reverse proxy for on-premises IPv6 services. 
If you have not already done so, give your URL to a CDN provider for administration, 
who will offer access via https as an additional CDN service if required. 
(One possible CDN provider with a free basic plan is Cloudflare ).

# Task 1 - Nginx
## (1b) Question : Why is it useful in many cases to block access to the web server using an IP address?
* Secure websites use certificates for verification and the certificates are primarily associated with domain names not IP address.

* And at any given time, the website can hosted under a new/different provider with another IP address anywhere in the world.
  
* In practice most web servers are usually accessed via a proxy/loadbalancer for security and efficient traffik distribution. Hence accessing the server directly with its Ip address defeats this objective.
.
## (1b)Provide your docker compose file and configuration file /etc/nginx/nginx.conf. Explain both files in detail.

```
services:
  reverse-proxy:
    container_name: reverse-proxy
    ports:
      - 80:80
    image: nginx:latest
    volumes:  # I used bind mount with exact reference to my docker-compose-5.1.yml directory
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro   # ro here defines read only
      - ./nginx/ssl:/etc/nginx/ssl
      - ./nginx/websites:/usr/share/nginx/html
    networks:
      my_ipv6:
        ipv6_address: 2001:638:408:200:ff6c:cafe::7777
      loc_ipv6:

    restart: unless-stopped

networks:    # both networks were already created externally
  my_ipv6:
    name: my_ipv6
    external: true
  loc_ipv6:
    name: loc_ipv6
    external: true

```

#

```
events {
worker_connections  4096;

}


http {
  include /etc/nginx/mime.types;
  sendfile on;

   server {
        listen 80;
        listen [::]:80;
        server_name "";

      location / {
          root /usr/share/nginx/html;
          index undefined.html;
      }
    }


  server {
    listen 80;
    listen [::]:80;
    server_name migbin2s-servemgmt.site  www.migbin2s-servemgmt.site;

    location / {
      root /usr/share/nginx/html;
      index index.html;
    }
  }


}

```

## (1d) Start and test your server.
My solution:

http://www.migbin2s-servemgmt.site

http://[2001:638:408:200:ff6c:cafe:0:7777]

# Task 2 - HTTPS server
## (2a) Enable CDN proxying for your server with the URL www.yourDomain.xy so that your server is reachable with http. Which IPv6 address is used to www.yourDomain.xy reach your server from the outside.

## (2b) Set up the CDN proxy to support access to your Nginx server with both http(= unsecured) and with https.

## (2c) How do you rate the security of your https web server?