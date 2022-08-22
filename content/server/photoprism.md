---
title: "Photoprism"
date: 2022-08-08T22:30:19-05:00
draft: true
---

photoprism setup  
https://docs.photoprism.app/getting-started/docker-compose/

set up photoprism on unraid and have the servers synced. currently, nebula is set to sync to unraid but unraid is not set to sync to nebula.  
the faces worker is set to run every 15 minutes  
it seems the sync worker is also set to run like 200 images at a time. not sure, need to read the docs  

nebula photoprism: the nextcloud webdav sync kind of is miserable. the photos are owned by root when transferred and i have no idea why one photo did and one photo did not transfer. i have since disabled it and will resort to manual (cron automated) rclone tasks. 


migrate primary photoprism from nebula to unraid

backup and restore:  
https://docs.photoprism.app/getting-started/advanced/backups/  
https://github.com/photoprism/photoprism/discussions/772  
rclone to migrate (rclone sync) nebula originals to unriad photoprism directory. should not be many if any changes.  
move photoprism-db.sql file 
point 
