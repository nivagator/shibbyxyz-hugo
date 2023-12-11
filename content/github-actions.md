---
title: "Deploy Static Site with Github Actions"
date: 2023-12-11T12:03:02-06:00
---

1. on the remote webserver, create deploy user
    ```bash
    useradd -G www-data deploy -m -d /var/www/site/root
    ```
  - -G, --groups GROUPS - list of supplementary groups of the new account
  - -m, --create-home - create the user's home directory
  - -d, --home-dir HOME_DIR - home directory of the new account

2. on your local machine, create ssh key pair for deploy user
    ```bash
    ssh-keygen -t ed25519 -f ~/.ssh/deploy
    Generating public/private ed25519 key pair.
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in ...
    Your public key has been saved in ...
    The key fingerprint is:
    ...
    The keys randomart image is:
    ...
    ```

3. create .ssh directory in the deploy user home directory and copy public key into authorized_keys file
  `mkdir /var/www/site/root/.ssh`  
  `vim /var/www/site/root/.ssh/authoirized_keys`

4. set ownership and permissions on webserver
   ```bash
   sudo chown -R deploy:www-data /var/www/site/root
   sudo chmod 700 /var/www/site/root/.ssh
   sudo chmod 500 /var/www/site/root/.ssh/authorized_keys
   ```

5. test ssh connection from local machine
6. mkdir -pv .github/workflows in project root
7. create ci.yml file
8. create deploy environment on github project
9.  add environment secrets
  - SSH_PRIVATE_KEY
  - DEPLOY_SERVER - server ip
  - DEPLOY_USERNAME - deploy username
  - DEPLOY_PORT (if needed)
          