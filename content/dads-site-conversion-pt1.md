---
title: "Dads Site Conversion | part 1"
date: 2022-05-08T14:55:32-05:00
tags: ['projects']
---

## ~~quick~~ backstory
the first version of my dad's website was built back in the early 2000s by a associate music minister at our church. it was largely just informational with a contact page for getting in touch. in college, i started to dabble more and more with programming and websites and sometime around 2015, i asked if i could rebuild/manage his site. 

the original version was built on a single page html template that i think i paid for? rookie mistake. it did have some cool parallax effects that i had not seen before. I was able to arm wrestle it into submission. This site lasted pretty much as is until 2022.

the other objective was to set up an online storefront to allow him to sell digital copies of his sheet music and audio tracks/albums. this was slightly more overwhelming. i looked at a lot of solutions but landed on the path of least resistance in wordpress and an e-store plugin (barf). this was ok, it doesn't look great, but it works and its integrated with paypal.

My web host at the time was with media temple on their managed grid service, so that's where these lived. over the years, i migrated every other web service i had (new stuff and old) of media temple and over to [linode](https://linode.com). mainly because i was getting into linux and the vps model was more hands on with more control. but dad's sites lingered on media temple and it was because of the store and its wordpress backend. migrating that seemed like a daunting task, so for years, i just left it alone. 

as an aside: Media Temple is hot, steaming garbage. maybe they weren't always terrible, but [back in 2013](https://mediatemple.net/blog/press/godaddy-acquires-mt-media-temple-to-accelerate-web-pro-expertise/) they were acquired by godaddy (which is also hot garbage) and that may have something to do with it. their support is a miserable experience and their "knowledge base" is absolute junk. linode on the other hand is the polar opposite. they have [recently been acquired by akamai](https://www.linode.com/press-release/akamai-to-acquire-linode/), so I'll be paying close attention to see if that impacts their service and/or support. this year, media temple announced they were raising the price of their grid service by 50%, which was already 4x the cost of a linode nanode. that was the last straw; i was determined to get off their servers for good. 

## the plan

### linode 
I knew i wanted to move to linode since my other services were already there. setting up a vps and hosting a website is something ive done many times using many different technologies. The wrench here was migrating an existing wordpress install, its database, et al. the nanode tier should be fine for the amount of traffic and data transfer needed and upgrading this in the future is simple. 

### new main site
i also knew it wanted to refactor the main website page. the existing html template was cumbersome to update since its just one contiguous file. plus, the template has a lot of garbage in it too that wasn't needed. 

my thoughts went to using a static site generator. ive used [jekyll](https://jekyllrb.com/) for my '[professional site](https://gavingreer.com/)'. i'm using [hugo](https://gohugo.io/) here and i previously ran this site off of [ssg](https://romanzolotarev.com/ssg.html). these are all marketed as blogging platforms, but i only wanted the ability to segment the content into its own files and dynamically script it if needed.

i decided to go with jekyll. it seemed like it had the lowest barrier to entry.

### container all the things
ive been learning a lot about docker and containerization through the [Self-Hosted Show](https://selfhosted.show/). There's a lot to like about the mobility baked in. If, in the future, i need to migrate again for whatever reason, this would provide a simple path. also, this might make backups easier to manage. the plan is to use existing docker images rather than construct my own dockerfile. wordpress, nginx, mysql and certbot all have images that i'll use. oh, and use docker-compose to manage it all. 

### domain control
the existing site places the store in a subdirectory. this is fine, the migration to a static site generator might not like that. another objective was to move the store page to a subdomain through dns management. 

### certs
another no-no im correcting is the current wordpress store is un-encrypted, http traffic. a must for the new site is to have it secured with ssl certs. 

### backups
i need to make backups happen. through the self-hosted show ive been exposed to restic. my plan is to use restic to make backups easy to backblaze.  

### git it together
everything i'm doing needs to be under git version control. easy. 

### cloudflare dns
using cloudflare is a bonus goal, but something i wanted to explore in more depth. no better time like the present.

## next

with the objectives laid out and technologies more or less chosen, it was time to start... reading. i found several articles for installing and/or migrating existing wordpress installs to docker. 

[digital ocean: install wordpress with docker compose](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose)  
[migrate wordpress to docker](https://codeblog.dotsandbrackets.com/migrate-wordpress-docker/)  
[moving wordpress into docker container](https://stephenafamo.com/blog/posts/moving-a-wordpress-site-into-a-docker-container)  
[digital ocean: wordpress subdomian with nginx and ssl](https://www.digitalocean.com/community/questions/how-to-make-a-wordpress-subdomain-on-nginx-with-ssl)  

### other resources 

[ed22519 ssh keys](https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54)   
[feross: linode inital set up ](https://feross.org/how-to-setup-your-linode)



