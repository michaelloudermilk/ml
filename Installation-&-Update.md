Welcome to the UNMS installation & update guide. UNMS can be deployed as a docker image on your own server. Please, choose your system and follow the instructions to install a fresh copy or update an existing installation.

## Linux

#### Prerequisites
- bash, curl
- 1GB RAM
- 8GB storage
- access to ports 80 and 443
- ping allowed (see Devices latency)

#### Install
Run this command on the host to install docker (it pulls the latest UNMS image and start it). If an UNMS installation already exists, it will be updated to the latest version. When done the UNMS can be found at [http://localhost/](http://localhost/).

    curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh

#### Cloud
We recommend using the latest version of Ubuntu or Amazon AMI. Examples of suitable cloud services:
- [AWS](https://aws.amazon.com/), EC2 instance, _t2.micro_ (1GB RAM) or bigger, Ubuntu
- [DigitalOcean](https://www.digitalocean.com), basic droplet (1GB RAM) or bigger, Ubuntu

#### UNMS data
By default, the installation script ensures that the application settings and data (logs, pictures, encryption key, etc.) will be stored outside the docker container (```/home/unms/data```). This will enable you to backup that data and more importantly, this will enable you to perform any future UNMS upgrades without any data loss.

#### SSL certificate
By default, UNMS uses [Let's Encrypt](letsencrypt.org) to create an SSL certificate for its domain name. The certificate is saved under ```/home/unms/data/cert/live```. It's possible to replace the certificate with your own if required.

#### Devices latency
By default, connected devices ping the UNMS host to check latency. This requires ping to be allowed on the UNMS host to work.

## Windows, OS X

Unfortunately, Docker does not provide full support for these systems. Thus we can't ensure smooth backups and upgrades. At this moment we recommend to install VirtualBox with latest version of Ubuntu and then follow the Linux instructions above.