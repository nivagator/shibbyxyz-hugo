---
title: "Lychee"
date: 2022-09-29T07:53:37-05:00
tags: ['linux']
---

I don't participate on social media sites, but I do want to be able to easily share public photos with friends. This site works, but i wanted something built for photos with album support and options to make some public or private. 

The Self Hosted show has talked a lot about photo sharing solutions. In [episode 28](https://selfhosted.show/28) they mention Lychee.
The Self Hosted show has talked a lot about photo sharing solutions. In [episode 28](https://selfhosted.show/28) they mention [Lychee](https://lycheeorg.github.io/). This is a very simple, open source photo album server that fit what I was looking for perfectly. 

As usual, I found a great docker implementation by the good people over at [linuxserver.io](https://docs.linuxserver.io/images/docker-lychee), complete with a `docker-compose` file which required very little tweaking.

```yaml
version: "3"
services:
  mariadb-lychee:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: lychee_mariadb
    restart: always
    volumes:
      - ./mariadb-lychee/data:/config
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_DATABASE=lychee
      - MYSQL_USER=lychee
      - MYSQL_PASSWORD=dbpassword
      - PGID=1000
      - PUID=1000
      - TZ=America/Chicago
  lychee:
    image: lscr.io/linuxserver/lychee:latest
    container_name: lychee
    restart: always
    depends_on:
      - mariadb-lychee
    volumes:
      - ./config:/config
      - ./pictures:/pictures
    environment:
      - DB_HOST=mariadb-lychee
      - DB_USERNAME=lychee
      - DB_PASSWORD=dbpassword
      - DB_DATABASE=lychee
      - DB_PORT=3306
      - PGID=1000
      - PUID=1000
      - TZ=America/Chicago
    ports:
      - 80:80
```

Easy, simple, no big deal. 

The trick as always is to get nginx and ssl working with the docker container. 

I sort of had a blueprint for how to set this up from my healthchecks deployment. It was easy to copy/paste/edit the config.

```nginx
server {
    listen 80;
	listen [::]:80;
	
    server_name photos.shibby.xyz;

	location / {
	    proxy_pass http://127.0.0.1:80xx; # lychee docker
	}
```

Yep, its that easy. Then all it needed was to be put through `certbot` to enable SSL and its done. 

Lychee is pretty great so far. Super easy to manage and control albums, sharing, users, etc. The minimalist interface is just what I wanted. It has a cool feature where it will import a photo given a direct link to the file. I can see this being useful to make grabbing images from the internet easily. It has fairly obvious dropbox integration, which i dont use, but I'm interested to see what other platforms the might be able to pull from.

Check it out [photos.shibby.xyz](https://photos.shibby.xyz).

## Update 2023-12-30
container updates will fail to load static resources. They will attempt to load from localhost. This requires an addition of a `proxy_set_header` to the nginx config for the site. 

```
location / {
    proxy_pass http://127.0.0.1:8088; # lychee docker
    proxy_set_header Host $host;
}
```
