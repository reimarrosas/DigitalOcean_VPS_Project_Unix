# Guide for Setting up the Server

Note: Everything inside braces `{ }` are placeholder names.

## Acquiring VPS from Digital Ocean

1. Sign up to [Digital Ocean](https://cloud.digitalocean.com/registrations/new).
2. Click `Create Virtual Machine` in the Welcome Page.
3. Select the following settings:
   1. Ubuntu 18.04 LTS x64.
   2. Shared CPU Basic.
   3. Regular Intel with SSD.
   4. $5.00/month.
4. Choose the Data Center closest to you.
5. Add your SSH Keys as more secure authentication when connecting to the
   website. If you don't have ssh keys, [generate them in Windows](https://phoenixnap.com/kb/generate-ssh-key-windows-10)
   or [generate them in Linux](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
6. Set your preferred root password.
7. Press `Create Droplet`.

## Setting up the VPS

1. Select your Droplet in the Home Page.
2. Click `Console` on the right most part of the bar underneath the `ON` toggle.
3. You will automatically be logged on as root.
4. Update the system with the command `apt update && apt upgrade`.
5. Restrict SSH login by change the following settings:
   1. Set `PermitRootLogin` to `no`.
   2. Set `PasswordAuthentication` to `no`.
   3. Set `UsePAM` to `no`.
6. Create a user using the command `adduser --ingroup sudo {USER NAME}`.
7. Change the password of the user using the command `passwd {USER NAME}`.
8. Install NginX as the web server using the command `apt install nginx`.
9. Enable NginX on startup using the command `systemctl enable nginx`.
10. Start the NginX service using the command `systemctl start nginx`.
11. Set the firewall settings using the following commands:
    1. `ufw default deny incoming`
    2. `ufw default allow outgoing`
    3. `ufw allow OpenSSH`
    4. `ufw allow 'Nginx Full'`
12. Check the added ufw rules using the command `ufw show added`. It should show
    that NginX and OpenSSH are allowed.
13. Enable the firewall using the command `ufw enable`.

## Setting up the Domain Name

1. Go to [Namecheap](https://www.namecheap.com) website.
2. Login to your Namecheap account. If you don't have an account, click the
   `Account` navigation menu, click the `SIGN UP` link, and follow the
   instructions.
3. Search for your desired website name without the top-level domain i.e.
   `mercatura` in the `Search for your next domain` search bar.
4. Press `Add to cart` button for your selected domain name.
5. Hover over the cart icon on the top-right most side of the web page and click
   view cart.
6. Press `Confirm Order` button and complete your purchase. Afterwards you will
   be redirected to the Dashboard.
7. To set up the DNS records so that the domain name will target your
   website, click on the `MANAGE` button.
8. Click the `Advanced DNS` menu.
9. On the `HOST RECORDS` section, if there's any Host Record already included,
   delete them.
10. Click `ADD NEW RECORD` button and add the following records. You can find
    the VPS IP in the same bar that contains the `Console` link indicated by
    `ipv4`.
    | Type | Host | Value |
    |--------------|------|---------------|
    | A Record | @ | {VPS IP} |
    | CNAME Record | api | {Domain Name} |
    | CNAME Record | www | {Domain Name} |

11. Click the `Save All Changes` button to save the DNS records.
12. After a few seconds, you should be able to use your domain name as URL for
    your browser and the NginX Default Home Page should show up.

## Setting up NginX

1. Go back to the Console Window of your VPS.
2. Navigate to `/etc/nginx/sites-available`.
3. Change the name of the `default` file to `old_default` to store it instead of
   deleting it using the command `mv default old_default`.
4. Create new files `default`, `{Domain Name}`, and `api.{Domain Name}` using
   the command `touch default {Domain Name} api.{Domain Name}`
5. Create symbolic links for each file to the `sites-enabled` directory using
   this command for each file `ln -s {File Name} ../sites-enabled/{File Name}`.
6. Edit the default file, enter the following code and then save:

   ```nginx
   # This code redirects all HTTP request to HTTPS
   server {
       listen 80 default_server;
       listen [::]:80 default_server;
       server_name _;
       return 301 https://$host$request_uri;
   }
   ```

7. Edit the base domain name file, enter the following code and then save:

   ```nginx
   # This code serves the static content for all domain name requests.
   server {
       listen 443 ssl default_server;
       listen [::]:443 ssl default_server;

       root /var/www/html;

       index index.html;

       server_name {Domain Name} www.{Domain Name};
   }
   ```

8. Edit the api domain name file, enter the following code and then save:

   ```nginx
   # This Code redirects all requests to the api domain name to localhost:1200
   server {
       listen 443 ssl;
       listen [::]:443 ssl;
       server_name api.{Domain Name};

       location / {
               proxy_set_header X-Forwarded-For $remote_addr;
               proxy_set_header Host $http_host;
               proxy_pass http://localhost:1200;
       }
   }
   ```

## Setup TLS/SSL Certificate and Automatic Certificate Renewal

1. Switch to your created user using the command `su {USER NAME}`.
2. Ensure that snap/snapd is up to date using the command `sudo snap install core ; sudo snap refresh core`.
3. Install Certbot, the program that automatically refreshes your Certificate,
   using the command `sudo snap install --classic certbot`.
4. Prepare the Certbot program using the command `sudo ln -s /snap/bin/certbot /usr/bin/certbot`.
5. Run the following command to get a certificate and automatically renew them
   every 12 hours `sudo certbot --nginx`.
6. To confirm that it worked, you should be able to go to `https://{Domain Name}`
   in your browser.

## Setup the Source Code

1. Navigate to the directory where you want to store the source code.
2. Clone the website repository using the command `git clone https://github.com/reimarrosas/mercatura.git`.
3. Removed the default NginX website using the command `sudo rm -rf /var/www/html`.
4. Create a symbolic link of the client-side code to NginX default static file
   directory using the command `sudo ln -s ./mercatura/client /var/www/html`
5. If you try going to the url `https://{Domain Name}`, the website should show
   up but it would not load properly since the back-end is not yet started.
6. Install Node Version Manager for the most up to date version of NodeJS using
   the command `wget -qo- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash`
7. Refresh your bash config using the command `source ~/.bashrc`.
8. Copy the default NVM directory. You can show the directory using the command
   `echo $NVM_DIR`.
9. Install node using the command `nvm install --lts`.
10. Install the pip for later scripts using the command `sudo apt install python3-pip`.
11. Install the PostgreSQL database using the following commands:
    1. `sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'`
    2. `wget --quiet -O - https://www.postgresql.org/media keys/ACCC4CF8.asc | sudo apt-key add -`
    3. `sudo apt-get update`
    4. `sudo apt-get -y install postgresql`
12. Switch to the postgres user using the command `sudo -iu postgres`.
13. Create a new role/user using the command `createuser --interactive --pwprompt`. Note: Don't forget to set a password and add all the prompted roles.
14. Create a database using the command `createdb mercatura -O {PG USER NAME}`
15. Go back to the previous user by pressing `CTRL + d`.
16. Install required NodeJS global packages using the command `npm i -g yarn typescript`.
17. Go to the server directory of the repository using the command `cd ./mercatura/server`
18. Install the necessary project packages using the command `yarn install`.
19. Navigate to the database folder using the command `cd ./src/database`.reimar
20. Run the initialization SQL script using the command `sudo psql postgres://{PG USER NAME}:{ROLE PW @localhost:5432/mercatura -f init.sql`.
21. Run the seed data using the command `sudo psql postgres:://{PG USER NAME}:{ROLE PW}@localhost:5432 mercatura -f seed.sql`.

## Setup the Scripts

1. Create a scripts folder in reimar

   ````bash
   #!/bin/bash

   PORT=1200 DATABASE_URL="postgres://reimar:reimar@localhost:5432/mercatura" TOKEN_SECRET="yZbdMBmg94vWy9QP%cqjMBD%8Jn*m#$PAoK@KAY4qmkYc!Raw9stMt49N@uqQTNvyBjl9^dr9kgwO1KA4eXKZlwFSvU@wx3rUOrkCgi7s%0KZaWDEPG4vlVr2@QrdEm" COOKIE_SECRET="SRW3lOX7vSFbg^gYdsGMw#dP7dZbFJjf@M#5m70esrFeI2aGQK&48aNJiDBl4RqA^g8zoO04yfHHpOUeP*IqRObB^ND#qOZKNf^o96upQYNPmx0BdOrPgOqD2s9LWEU" /home/reimar/dev/mercatura/server/dist/app.jsreimar
   ```python
   #!/usr/bin/env python3

   from checksumdir import dirhash
   import os

   def main():
      directory = "/home/reimar/dev/mercatura"
      server_dir = "/home/reimar/dev/mercatura/server"
      orig_hash = dirhash(directory, "sha1")
      os.system(f"git -C {directory} pull")
      new_hash = dirhash(directory, "sha1")
      if (orig_hash != new_hash):
         os.system(f"killall node && yarn --cwd {server_dir} compile && systemctl start start-server.service")

   if __name__ == "__main__":
      main()reimar
   ````

2. Make the two files executable using the command `chmod +x {FILE NAME}`.
3. Change user as root using the following command `sudo su`.
4. Navigate to the directory for systemd services using the following command `cd /etc/systemd/system`.
5. Create the following files `server_start.service`, `update_repo.service`, and `update_repo.timer` using the following command `touch {FILE NAME}`.
6. Edit `server_start.service` and add the following:

   ```
   [Unit]
   Description=Starts the node server on startup

   [Service]
   ExecStart=/home/{USER NAME}/scripts/run_node

   WorkingDirectory={CLONE DIR}/server
   StandardOutput=syslog
   StandardError=syslog
   User=reimar

   [Install]
   WantedBy=multi-user.target
   ```

7. Edit `update_repo.service` and add the following:

   ```
   [Unit]
   Description=Simple CI/CD for the Mercatura App Backend

   [Service]
   ExecStart=/home/{USER NAME}/scripts/update_repo.py
   ```

8. Edit the `update_repo.timer` and add the following:

   ```
   [Unit]
   Description=Run update_repo.service every 10 minutes
   Requires=update_repo.service

   [Timer]
   OnUnitActiveSec=10min
   Unit=update_repo.service

   [Install]
   WantedBy=timers.target
   ```

9. Install `checksumdir` package via pip using the following command `python3 -m pip install checksumdir`.
10. Create a symbolic link for `node`, `tsc`, and `yarn` in the `/usr/local/bin` directory using the following command `ln -s {NVM DIR}/versions/node/{NODE VERSION}/bin/{FILE NAME} /usr/local/bin`. You can just autocomplete the node version with `TAB` since you only installed one node version.
11. Automatically start the `server_start.service` and `update_repo.timer` using the following command `sudo systemctl enable {FILE NAME}`.
12. Start the aforementioned service/timer using the following command `sudo systemctl start {FILE NAME}`.

# Finished

After all of the steps, you should have a fully-featured working web server. Congratulations!
