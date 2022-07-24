---
title: "Dads Site Conversion | part 2"
date: 2022-05-09T16:23:24-05:00
tags: ['projects']
draft: true
---

## linode procurement 

linode makes spinning up a new instance very simple. i've used [feross' linode inital set up guide](https://feross.org/how-to-setup-your-linode) for a few years. i deviate from it, but its something i use as a reference. i created a new instance, set up the basics, installed docker. 

## domain management 

I'm using two domains for this project. one is live and one is used as a test stand-in while the new site is in development. first i migrated both domains to use cloudflare dns. this was as simple as working through the cloudflare wizard and pointing the name servers as directed through godaddy and namecheap. this continued to point the live domain and the old site for the time being but it will be ready for the migration to the new site through cloudflare. 

i then set up the subdomain on the test domain name. as i mentioned, part of this project is moving the store from a subdirectory to a subdomain. i created two test. and store. for future use. 

## getting started on the new server 

the project directory will have the following structure:
```
~/docker/
    ├── website/
    ├── certbot/
    ├── db/
    ├── docker-compose.yml
    ├── .env
    ├── .git/
    ├── .gitignore
    ├── nginx-conf/
    ├── README.md
    └── wordpress-store/
```

### nginx conf
following the [DO guide](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose), i started by creating the nginx.conf file in the nginx-conf directory. i wont go through the guide line by line, but the key here is it points the .php requests at the wordpress docker container on port 9000. so far its only listening for http on port 80. it has locations for the certbot challenges as well. I'm using the test domain name here, its not yet at a subdomain.

this was actually a really cool way to run a webserver. in the past, i've always run nginx on the host. all server listen definitions are in a single file. 

### env
next we set up the database environment variables file and initialize a git repo being sure to add the `.env` file to the `.gitignore` and `.dockerignore` files. 

### container definitions 
then the fun begins with the `docker-compose.yml` file. each service (database, wordpress, nginx and certbot) are added with the appropriate settings. the keys here are the `app-network` definition which allows the containers to communicate internally on a docker network bridge rather than using external requests. 

### what could go wrong
i tweaked the appropriate settings and brought everything up. the containers were functioning fine, but i was getting a too many redirects error. after some searching, i found this was [due to cloudflare's SSL/TLS](https://themebeez.com/blog/fix-err-too-many-redirects-cloudflare-loop/). changing the cloudflare security level to Full(Strict) took care of it. 

the certbot container comes up, runs its commands and then exits with a status. after its up and running, you validate that the certificates have been mounted to the webserver. with this complete, you edit the `docker-compose.yml` file to replace `--staging` with the `--force-renewal` flag in the certbot command block. restarting the containers will get live certs. 

the next step are simple updates to the `nginx.conf` file and `docker-compose.yml` to account for the https traffic. again, recreating the containers and its done. 

at this point, i have a functioning docker compose stack including a fresh/bare wordpress site (pointed at the root domain), mysql database, nginx webserver and certbot ssl using cloudflare dns. the next challenge will be to put the wordpress site at a subdomain and the main site at the root. this will require a bit of gymnastics to get the certbot container to register the new without messing with the existing.

### docker volumes

the guide calls for using docker volumes. while i sort of get these in theory, they seem too abstract for what i'm doing here. plus, since im moving existing content to this wordpress install, i need access to the site data without jumping through a bunch of hoops. 

converting these was fairly simple using a docker run command.  
`docker run --rm --volumes-from {container name} -v {host backup location}:/backup ubuntu tar cvf /backup/{backup-name}.tar {path inside container. see compose for exact location}`  

this command creates a new container, mounts the volume, compresses it to a location on the host. once thats complete, decompressing the content to the host directory and updating the `docker-compose.yml` file to use the directories instead of the volumes and its done. 

### subdomain

the next step is to move the wordpress site to a subdomain and host a static directory on the root. i created the `website` directory, added the bind mount to the webserver and certbot container definitions in the `docker-compose.yml` file. the certbot container is interesting when [controlling multiple domains](https://eff-certbot.readthedocs.io/en/stable/using.html#webroot). each domain (and subdomain) need separate `--webroot-path` and `--domain` instances in the certbot command section. 

referring back to the DO guide, i tried flipping back to the `--staging` flag and removing the `--force-renew`. this led to a certbot container error `exit code 1` saying it already has certs for requested domains. 

here's what i came up with to add a new domain to the set of existing in the certbot container definition. 
- remove the domains already requested from certbot command
- add `--webroot-path` and `--domain` name for new domains
- run with `--staging` flag (remove `--force-renew`). 
- verify exit code 0 and the certificates exist with the following commands:
    - `docker-compose logs certbot`
    - `docker-compose exec webserver ls -la /etc/letsencrypt/live`
- replace original command block including all webroot-paths and domains as well as the `--force-renew` flag
- run recreate on certbot: `docker-compose up --force-recreate --no-deps certbot`
