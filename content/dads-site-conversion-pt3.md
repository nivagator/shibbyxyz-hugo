---
title: "Dads Site Conversion | part 3"
date: 2022-05-17T07:15:17-05:00
tags: ['projects']
---

# migration

Now that our server and domains are set up the total unknown of migrating a wordpress site from a host install to a docker container. 

here's what we need from the current site.

- A zip archive of the entire contents of the WordPress site - site.zip
- A dump of the MySQL database - database.sql
- The database name - example_db_name
- The database password - example_db_password
- The database user - example_db_user

## site content
the site content should extracted and placed in the directory defined in your docker compose file for the wordpress container. in my case, `~/docker/wordpress/`. once extracted, edit the `wp-config.php` file. insert the database name, user and password. 

the DB_HOST can accept the name of the mysql docker container as defined in the docker compose file. the ability to this is an advantage of running services on a bridged docker network. in my case, the container name is simply `'db'`. 

```php
/** The name of the database for WordPress */
define('DB_NAME', 'wp_store');

/** MySQL database username */
define('DB_USER', 'wpadmin');

/** MySQL database password */
define('DB_PASSWORD', 'password1');

/** MySQL hostname */
define('DB_HOST', 'db');
```

just above /* That's all, stop editing! Happy blogging. */, add the following:

```php
if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) 
    && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
    $_SERVER['HTTPS'] = 'on';
}
```

## database
same story here. in your docker compose mysql container definition, there are a few bind mounts: `/var/lib/mysql` and `/docker-entrypoint-initdb.d`. in my case, i point these to `~/docker/db/data` and `~/docker/db/initdb.d` on the host respectively. 

as long as the data directory is empty, the mysql container will execute any .sh, .sql or .sql.gz file in the /docker-entrypoint-initdb.d folder inside the container. this is where to place the sql dump.

add database name, password and user to the docker compose file, either directly through and environment file. I'm using an .env file. both the db and wordpress containers need additions.

```bash
- db: 
    ...
    environment: 
        - MYSQL_DATABASE=wp_store 
    volumes:
        - ./db/data:/var/lib/mysql
        - ./db/initdb.d:/docker-entrypoint-initdb.d
- wordpress:
    ...
    environment: 
        - WORDPRESS_DB_HOST=db:3306
        - WORDPRESS_DB_USER=$MYSQL_USER
        - WORDPRESS_DB_PASSWORD=$MYSQL_PASSWORD
        - WORDPRESS_DB_NAME=wp_store


```

> thats it. the site has been migrated BUT it wont load all assets correctly. 

## config and database url corrections

the database and wordpress are connected, and the home page loads, but links dont work because they refer to the old domain and subdirectory. 

This will require changes to the database. [This blog post](https://codeblog.dotsandbrackets.com/migrate-wordpress-docker/) had what i needed. these queries worked for me be sure to not include a trailing slash here. 

```sql
UPDATE wp_options SET option_value = replace(option_value, 'http://www.oldurl', 'http://www.newurl') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'http://www.oldurl','http://www.newurl');
UPDATE wp_posts SET post_content = replace(post_content, 'http://www.oldurl', 'http://www.newurl');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'http://www.oldurl','http://www.newurl');
```

After these updates, i was able to access the wp-admin page.

I had other database updates to make on the e-store plugin side, those were relatively painless once found all refereces. after this, the product links and pages work but the thumbnails still do not load. again after some searching, i found that the e-store plugin being used was using a utiliy to generate thumbnails called TimThumb. i changed the cache folder permissions and it worked, but i had doubts that this was a long term solution. 

all of these steps are replacing the url with the test domain name and subdomain. I'll have to make these same database adjustments when the site goes live. 

other issues include:
- the store is still connected to the live paypal account
- wordpress can't auto update. might need a ftp solution in docker?
- the e-store plugin isnt sending emails correctly
- i want to look into paypal's return function