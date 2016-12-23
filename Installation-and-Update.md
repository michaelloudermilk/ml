Welcome to the UNMS installation and update guide. UNMS can be deployed as a docker image on your own server. Please choose your system and follow the instructions to install a fresh copy or update an existing installation.

## Linux

#### Prerequisites
- bash, curl
- 1 GB RAM
- 8 GB storage
- access to ports 80 and 443
- ping allowed (see Devices latency)

#### Install
Run this command on the host to install docker (it pulls the latest UNMS image and starts it). If a UNMS installation already exists, it will be updated to the latest version. When the process is complete, you can access UNMS at [http://localhost/](http://localhost/). You can register your devices to UNMS using this tutorial: [[Register Devices to UNMS]].

    curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh

#### Cloud
We recommend using the latest version of Ubuntu or Amazon AMI. Examples of suitable cloud services:
- [DigitalOcean](https://www.digitalocean.com), basic droplet (1GB RAM) or bigger, Ubuntu
- [AWS](https://aws.amazon.com/), EC2 instance, _t2.micro_ (1GB RAM) or bigger, Ubuntu

_Note: There is a detailed tutorial for DigitalOcean: **[[UNMS on DigitalOcean]]**._

#### UNMS data
By default, the installation script ensures that the application settings and data (logs, pictures, encryption key, etc.) will be stored outside the docker container (```/home/unms/data```). This will enable you to back up that data, and more importantly, this will enable you to perform any future UNMS upgrades without any data loss.

#### SSL Certificate
By default, UNMS uses [Let's Encrypt](letsencrypt.org) to create an SSL certificate for its domain name. The certificate is saved under ```/home/unms/data/cert/live```. You can replace the certificate with your own if required to do so.

#### Devices Latency
By default, connected devices ping the UNMS host to check latency. Ping must be allowed on the UNMS host for UNMS to work.

## Windows, OS X

Unfortunately, Docker does not provide full support for these systems, so we can't ensure smooth backups and upgrades. At this time we recommend that you install VirtualBox with the latest version of Ubuntu and then follow the Linux instructions above.