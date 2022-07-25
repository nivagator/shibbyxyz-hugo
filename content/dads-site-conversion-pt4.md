---
title: "Dads Site Conversion | part 4"
date: 2022-05-22T07:55:57-05:00
tags: ['projects']
---

## wordpress auto updates

after attempting and failing and reading about the security risks of running and ftp instance, i did some reading on the alpine image under the wordpress container im using. the default user is 82. if wordpress detects the host user and the container user are not the same, it will ask for ftp credentials, otherwise it will update automatically.  

[wordpress: auto background updates](https://wordpress.org/support/article/updating-wordpress/#automatic-background-updates)  
[docker running as user](https://hub.docker.com/_/wordpress/) - running as arbitrary user section

so i read about [defining users in docker containers](https://blog.giovannidemizio.eu/2021/05/24/how-to-set-user-and-group-in-docker-compose/). setting `user: 1000:1000` on the wordpress container definition allowed auto updates to work since the host user was also `1000:1000`.

## wordpress email

the built in wordpress mail was not working. i installed WP Mail SMTP plugin and successfully mapped it to a new address, with my mail host. it is working and successfully sending emails from the e-store admin page but transaction confirmation emails are not being sent. This could be due to sandbox mode, but i have my doubts. 

## paypal sandbox and return

the migrated site was still connected to my dad's actual paypal account. i switched the site into sandbox mode and connected it to his developer account. 

i created a PDT token with paypal, defined the return url. this allows paypal to return customers to the website for instant delivery of download links. This should help by reducing reliance on the email delivery.  

everything seems to be working fine, but transaction confirmation emails are still not being sent. 
