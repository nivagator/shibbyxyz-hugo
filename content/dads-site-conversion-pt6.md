---
title: "Dads Site Conversion | part 6"
date: 2022-06-06T08:36:00-05:00
tags: ['projects']
---

to finish the migration, i started by pointing the `store` subdomain of the production domain at the new store. it worked no problem. this means i have:
- store.{domain}.com pointing at the new wordpress store
- {domain}.com/store pointing at the old wordpress store 
- {domain}.com pointing at the old server html site. 

next, the wp admin settings, address url and site address url were updated to match the new subdomain url. again this worked. 

the saved mysql url update queries were modified with the new domain and executed no problem. I did this by opening a port for mysql connections in the docker container, connecting it to an adminer instance to execute the queries, then closing the port. I have no need for external access to the database. after queries, products have correct urls, but images do not load due to the tim thumb site restriction.  

a quick update to timthumb.php and images load correctly:  
`/wp-content/plugins/.../lib/timthumb.php`  
line 215: `$this->myHost = preg_replace('/^www\./i', '', 'store.{domain}.com');`  

the store.{domain}.com url is fully functional at the new wordpress store. 

## backups

I then turned my attention to backups. I created a bucket on backblaze, and used restic. 

```bash
restic -r b2:e-store-server:linode --verbose backup ~/docker
```

this was so easy. there were some permission issues with some of the database data. `sudo` can correct this, but i need to find a solution that doesnt require `sudo`.

## next

reconfigure paypal settings and run live test transactions. 