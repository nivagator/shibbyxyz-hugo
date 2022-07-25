---
title: "Dads Site Conversion | part 8"
date: 2022-06-20T16:10:45-05:00
tags: ['projects']
---

## status

After a few weeks, everything seems to be functioning without issue. Dad reports he's getting fewer emails from customers asking for renewed download links. I'm finally off of media temple's service. The site loads faster, is more portable, i have control and incremental backups for a quarter of the cost. 

In a little under a month, i was able to knock out this project. glad to have it behind me and glad to have it documented and in a state that if i need to make changes, i can without much worry that i'll nuke it. 

## updates

i successfully updated wordpress to v6.0 through the web interface. ran server updates and rebooted. all docker services came back up just as expected with no issues. 

## issues to figure out in the future

the e-store doesn't write sales data to the database. this was a problem before the migration, but its not something we've ever needed to fix. its on my list though. im not terribly inclined to troubleshoot this since all other functions work as intended where the database is concerned so it very well could be an issue with the plugin itself? 

i still need to automate and/or correct the restic permission errors.

# DONE

I'm considering this project complete. this was a huge win. 