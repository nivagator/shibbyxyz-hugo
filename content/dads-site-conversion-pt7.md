---
title: "Dads Site Conversion | part 7"
date: 2022-06-07T16:02:24-05:00
tags: ['projects']
---

dad had to reset his paypal password so i couldn't get started with that right away. 

## style and content

I started by making a lot of css updates to the jekyll site. updated his bio, changed the header section with css media keys to make it 100% height on phones and mobile devices. 

## paypal configuration

i updated the paypal settings in the e-store to the production account and disabled sandbox mode. i created a test product for $0.01 and ran live transactions successfully. the site was correctly redirecting successful payments back to the site where instant download links were available. this is a huge win. 

## wordpress email

emails: not so much. still not sending emails when transactions go through. i read around and looked at a couple of wordpress smtp plugins. I settled on Post SMTP and i finally got the settings right and it was sending emails; both notification to the seller and confirmations to the buyer. emails are text only/no html, which is fine with me. 

## go live

at this point, everything is ready to go, it just needs the big switch flipped. i waited until late, after the kids were down to migrate. probably 10 minutes in cloudflare to point the domain to the linode server and it was done. domains are pointing correctly and all traffic is being routed to the linode vps. no transactions that evening, but i was on high alert watching the traffic and the inbox for any sign of issues. 

