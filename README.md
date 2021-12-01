# VPS Deployment for Web Application

## Goal

The goal of this project is to demonstrate how to get a Virtual Private Server
and modify it to host a Web Application.

## Choice of VPS

To deploy on a VPS, we must first choose what VPS we must use. After
deliberation, I narrowed it down to two VPS based on support and price.

* [Digital Ocean](https://www.digitalocean.com/)
* [Amazon Web Services](https://aws.amazon.com/)

### Digital Ocean

#### DO Pros

* Popular web hosting service for smaller companies
* Easy to setup
* Incredible official and community documentation
* Predictable pricing

#### DO Cons

* Smaller company than Amazon
* A bit harder to scale than AWS
* Not as fully-featured as AWS
* Not as prevalent as AWS

### Amazon AWS

#### AWS Pros

* THE number most popular Infrastructure service in the world
* Built to scale
* Industry standard
* A lot of related services in one place

#### AWS Cons

* Built with scale in mind
* Not as easy to get into as Digital Ocean
* Much more expensive on the scale that I'm working with (Single website with
  little to no traffic)

## Choice of Web Server

For serving the static contents and domain/port redirection, we need to use a
web server. I narrowed it to the two most popular web servers.

* [Apache Web Server](https://httpd.apache.org/)
* [NginX](https://www.nginx.com/)

I chose NginX since Apache has a ton of features that I frankly do not need and
will just cause unnecessary bloat like. I only need it as a Reverse Proxy and
to receive HTTP requests and I didn't use PHP as my backend. Therefore, a
simpler web server should be an obvious choice but based on what I read
online, there's not that much difference because of leveraging various load
balancing techniques like CDN, proxies, etc.

## Technologies Used

* [Digital Ocean](https://www.digitalocean.com/) as VPS to host the server
* [NginX](https://www.nginx.com/) as web server
* [UncomplicatedFirewall](https://wiki.ubuntu.com/UncomplicatedFirewall) Simple
  firewall for managing ports and connections
* [systemd](https://systemd.io/) for timer scripts and handling startup services
* [Github](https://github.com) as remote repository for this report and the
  application source code
* [Namecheap](https://www.namecheap.com/) for domain name registration
* [Let's Encrypt](https://letsencrypt.org/) for TLS/SSL certificate and
  automatic certificate renewal

## Documentations Used

* [Initial Droplet Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04)
* [Firewall Setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)
* [NginX HTTP -> HTTPS Redirection](https://linuxize.com/post/redirect-http-to-https-in-nginx/)
* [NginX Reverse Proxy for NodeJS API](https://www.tecmint.com/nginx-as-reverse-proxy-for-nodejs-app/)
* [Automatic Certificate Renewal](https://certbot.eff.org/instructions?ws=nginx&os=ubuntu-18)
