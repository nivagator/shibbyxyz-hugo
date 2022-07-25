---
title: "Dads Site Conversion | part 5"
date: 2022-05-30T08:20:26-05:00
tags: ['projects']
---

## static main site

to this point, all of my attention has been on the wordpress store. i rebuilt the site using jekyll and the creative theme. 

this is the index layout now. it has the illusion of being much more managable just by segmenting the sections into their own html files. 

```html
<!DOCTYPE html>
<html lang="en">

{% include head.html %}

<body id="page-top">
  {% include nav.html %}
  {% include header.html %}
  {% include meet-bruce.html %}
  {% include connections.html %}
  {% include portfolio.html %}
  {% include store.html %}
  {% include contact.html %}
  {% include scripts.html %}
</body>

</html>
```

i had some trouble getting the css file to propogate. my browser is caching the file and will not refresh it. i tried a ton of solutions, but none of them worked except one, very hacky, fix. i saw it called 'cache spoofing'. you add a query string to the css link in the head of the page file. it can be anything, arbitrary. I still dont have an actual solution to this. you can supposedly disable caching in the network tab of the developer options but not even this works in my case. 

```html
 <link rel="stylesheet" href="css/main.css?fi3f9vaefkl342" type="text/css">
```

the site is completely reconstructed and live on the site and i'm happy with it.  

## still to go

- point domain dns to the new server
- update nginx config file to migrate between the domain names
- reissue certbot
- open mysql port for access through adminer, close port once complete
- run database url update queries where old url = test domain and new url = live domain
- update wordpress site urls
- update website links in html
- update website links in jekyll site
- update e-store paypal address, PDT key to production accounts and disable debug mode
- figure out wordpress mail
