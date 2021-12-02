# Guide for Setting up the Server

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
7. Change the password of the user  using the command `passwd {USER NAME}`.
8. Install NginX as the web server using the command `apt install nginx`.
9. Set the firewall settings using the following commands:
    1. `ufw default deny incoming`
    2. `ufw default allow outgoing`
    3. `ufw allow OpenSSH`
    4. `ufw allow 'Nginx Full'`
10. Check the added ufw rules using the command `ufw show added`. It should show
    that NginX and OpenSSH are enabled.
11. Enable the firewall using the command `ufw enable`.
