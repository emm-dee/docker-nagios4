# Nagios 4 with SSL/TLS -- Docker Container

**Fork of: https://github.com/guessi/docker-nagios4**
## **Changes from Forked Source:**
* Enforced 443 
* Edit to redirect / to /nagios (apache)
* Merged some run commands for fewer layers
* Removed NRPE Server (check_nrpe is still here of course)

## **Container Mounts:**
* You'll want to mount your nagios config to `/opt/nagios/etc` inside the container
* Unless you rebuild the image injecting your certs, you'll need to mount yours to `/etc/ssl/cert.cer` and `/etc/ssl/cert.key`
* You can also make `/opt/nagios/var` a volume as well since Nagios uses it for stateful stuff. pid file and log will go there. 


## **Certs are REQUIRED.**
You can add them as a volume (shown in example below) 

**OR**

you can burn them into the image by editing the `Dockerfile` (see near the bottom of it for an example) and rebuilding an image from it. 


## **Example Run:**
Mounting cert/key and both the var and etc config dir. 
```
docker run -d \
  --name nagios \
  -p 443:443 \
  -v /path/to/nagios/data/etc:/opt/nagios/etc \
  -v /path/to/nagios/data/var:/opt/nagios/var \
  -v /path/to/certs/cert.cer:/etc/ssl/cert.cer \
  -v /path/to/certs/key.key:/etc/ssl/cert.key \
  mdebord/nagios4:latest
```

## **Accessing Nagios:**
Access it on straight https wherever you're running it. 

Username: `nagiosadmin`
Password: `adminpass`

Update these as you further customize your config. 

## **Don't have any config to mount?** - Generate some! 
You can temporarily run the container and copy the default config to your local dir: 

For Example: 
```
docker run -dit --name tempcontmd mdebord/nagios4:latest && docker cp tempcontmd:/opt/nagios/etc ./ && docker rm -f tempcontmd
````

