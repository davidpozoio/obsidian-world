Nginx is a reverse proxy, in simple words is a proxy for the server
![[proxies.excalidraw|700]]
This is useful to hide our service, and with this approach we can have multiples servers running in our vps and handle them using a reverse proxy, we have cache approach, and a more easy way to manage ssl certificates.
# Installation
in linux you can use the next command
```bash
sudo apt install nginx
```
Reload command
```bash
nginx -s reload
```
Test if config file is correct
```bash
nginx -t
```
# Setup
When we install nginx this will create a bunch of config files in the `/etc/nginx` folder.
The most important file is `nginx.conf` by default it has some content, we will remove this to start from scratch.
![[Pasted image 20250821223917.png]]
## Set the user
we need to specify by first what user we will use
```nginx
user root;
```
## Include mimetypes
we create our first block `http` and include the mimetypes statement, this is important because if we don't specify this our browser will detect our returned data as plain text
```nginx
http {
	include mime.types;
}
```
## Defining services
This step is important if we want to have different servers in a unique vps.
```nginx
# inside http block
upstream service_name {
	server 127.0.0.1:8000;
}

upstream other_service {
	server 127.0.0.1:8001;
}
```
## Redirect from 80 (http) to 443 (https)
Inside the http block we define this, this is important because we don't want users use the http part of our application by security reasons.
```nginx
# inside http block
server {
	listen 80;
	server_name domain_name1, domain_name2;
	
	return 301 https://$host$request_uri;
}
```
## Https server configuration
We need to specify a server block for each domain, this is because we need to set the ssl certificates keys, to a valid ssl certificate we can use [[Let's encrypt]].
```nginx
# inside http block
server {
	listen 443 ssl;
	server_name domain_name1; # specify the domain to match
	
	ssl_certificate /etc/letsencrypt/live/domain_name1/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/domain_name1/privkey.pem;
	
	location / {
		proxy_pass http://service_name;
	}
}
```
## Disable load any service using the ip server
If we want to avoid that users load our page using the ip of the server (for security reasons or hide the ip of our server), we can use this.
```nginx
# inside http block and before the server 80 redirect
server {
	listen 80 default_server;
	listen 443 default_server;
	
	ssl_reject_handshake on; # the response for the 443 port
	return 444; # the response for the 80 port
}

```