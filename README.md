# VPS Deployment and Hosting for Web Application

## Goal

The goal of the project is to demonstrate the student's (yours truly) knowledge
in basic system administration tasks by deploying and hosting a web application
in a Virtual Private Server.

## Choice of VPS

To deploy on a VPS, we must first choose what VPS we must use. After
deliberation, I narrowed it down to two VPS based on support and price.

- [Digital Ocean](https://www.digitalocean.com/)
- [Amazon Web Services](https://aws.amazon.com/)

### Digital Ocean

#### DO Pros

- Popular web hosting service for smaller companies
- Easy to setup
- Incredible official and community documentation
- Predictable pricing

#### DO Cons

- Smaller company than Amazon
- A bit harder to scale than AWS
- Lacks truly powerful features of AWS such as machine learning, Amazon powered
  analytics, etc.
- Not backed by world-renowned corporations like in AWS (Adobe, FINRA, AirBnB)

### Amazon AWS

#### AWS Pros

- THE number most popular Infrastructure service in the world
- Built to scale
- Industry standard
- A lot of related services in one place

#### AWS Cons

- Built with scale in mind
- Not as easy to get into as Digital Ocean
- Much more expensive on the scale that I'm working with (Single website with
  little to no traffic)

## Choice of Web Server

For serving the static contents and domain/port redirection, we need to use a
web server. I narrowed it to the two most popular web servers.

- [Apache Web Server](https://httpd.apache.org/)
- [NginX](https://www.nginx.com/)

I chose NginX since Apache has a ton of features that I frankly do not need and
will just cause unnecessary bloat. I only need it as a Reverse Proxy and
to receive HTTP requests and I didn't use PHP as my backend. Therefore, a
simpler web server should be an obvious choice. Based on what I read
online, however, there's not that much difference because of leveraging various load
balancing techniques like CDN, proxies, etc.

## Technologies Used

### Web Server

- [Digital Ocean](https://www.digitalocean.com/) as VPS to host the server
- [NginX](https://www.nginx.com/) as web server
- [UncomplicatedFirewall](https://wiki.ubuntu.com/UncomplicatedFirewall) simple
  firewall for managing ports and connections
- [systemd](https://systemd.io/) for timer scripts and handling startup services
- [Github](https://github.com) as remote repository for this report and the
  application source code
- [Namecheap](https://www.namecheap.com/) for domain name registration
- [Let's Encrypt](https://letsencrypt.org/) for TLS/SSL certificate
- [Certbot](https://certbot.eff.org/) for automatic certificate renewal

### Application

- [HTML5](https://html.spec.whatwg.org/multipage/)
- [CSS3](https://www.w3.org/TR/CSS/#css)
- [JavaScript](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)
- [TypeScript](https://www.typescriptlang.org/)
- [NodeJS](https://nodejs.org/en/)
- [Express](https://expressjs.com/)
- [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)
- [Argon2](https://github.com/ranisalt/node-argon2)
- [PostgreSQL](https://www.postgresql.org/)

## Documentations Used

- [Initial Droplet Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04)
- [Firewall Setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)
- [NginX HTTP -> HTTPS Redirection](https://linuxize.com/post/redirect-http-to-https-in-nginx/)
- [NginX Reverse Proxy for NodeJS API](https://www.tecmint.com/nginx-as-reverse-proxy-for-nodejs-app/)
- [Certificate Acquisition and Automatic Renewal](https://certbot.eff.org/instructions?ws=nginx&os=ubuntubionic)
