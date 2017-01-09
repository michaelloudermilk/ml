Welcome to the UNMS installation and update guide. UNMS can be deployed as a docker image on your own server. Please choose your operating system and follow the instructions below to install a fresh copy or update an existing installation of UNMS.

## Linux

#### Prerequisites
- Supported Distros: [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) and [Debian 8](https://www.debian.org/releases/stable/)
- 1 GB RAM (Minimal)
- 8 GB storage (Minimal)
- Local Ports: 80 and 443
- Allow ping (see Devices Latency and Outage Statistics)
- bash, curl

#### Installation Instructions
Run the command below on the host to install docker (it will pull the latest UNMS image and will start UNMS). If a UNMS installation already exists, it will be updated to the latest version. When the process is complete, you can access UNMS at [http://localhost/](http://localhost/). You can register your devices to UNMS using this tutorial: [[Register Devices to UNMS]].

    curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh

#### Cloud
We recommend using the latest version of [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) or Amazon AMI. Here are examples of suitable cloud services:
- [AWS](https://aws.amazon.com/), EC2 instance, _t2.micro_ (1 GB RAM), Ubuntu 16.04.1 LTS (Xenial Xerus)
- [DigitalOcean](https://www.digitalocean.com), basic droplet (1 GB RAM), Ubuntu 16.04.1 LTS (Xenial Xerus)

_Note: There is a detailed tutorial for DigitalOcean: **[[UNMS on DigitalOcean]]**._

#### UNMS Data
By default, the installation script ensures that the application settings and data (logs, site images, encryption key, etc.) will be stored outside of the docker container (```/home/unms/data```). This will enable you to back up that data, and more importantly, this will enable you to perform any future UNMS upgrades without any data loss.

#### SSL Certificate
By default, UNMS uses [Let's Encrypt](https://letsencrypt.org/) to create an SSL certificate for its domain name. The certificate is saved under ```/home/unms/data/cert/live```. You can replace the certificate with your own if required to do so.

#### Devices Latency and Outage Statistics
By default, all connected devices to UNMS will ping the UNMS host to check for latency and if any devices are being reported as offline which will result in outage statistics being generated.
Ping must be allowed to the UNMS Host for this to properly work.

## Windows, OS X

Unfortunately, Docker does not provide full support for these systems, so we can't ensure proper operation of UNMS, smooth backups and upgrades. At this time we recommend that you install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) with the latest version of [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) and then follow the Linux instructions above.