It's a service that give us ssl certificate for free, we can use certbot to create this certificates.
# Certbot
this is a tool to create let's encrypt certificates more easily.
## Nginx
certbot detects the domains in our nginx.conf file and it will create the ssl certificates. But by default this is going to add the configuration in the nginx.conf file too, but if we want to avoid this we can use the next command
```bash
certbot certonly --nginx
```
this is going to analyze the config file of nginx but it won't add an automatic generated configuration.
## Folders
Certbot creates the ssl certificates in the same folder `/etc/letsencrypt/live`, here we have a folders with the names of our domains `etc/letsencrypt/live/domain_name`.
The domain folder have two important files `fullchain.pem` and `privkey.pem` one is the ssl certificate and the other is the ssl key.
