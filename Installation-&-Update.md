Welcome to the UNMS installation and update guide. UNMS can be deployed as a docker image on your own server. Please choose your operating system and follow the instructions below to install a fresh copy or update an existing installation of UNMS.

## Linux

#### Prerequisites
- Supported Distros: [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) 64-bit and [Debian 8](https://www.debian.org/releases/stable/) 64-bit
- 1 GB RAM (Minimal)
- 8 GB storage (Minimal)
- 64-bit (x64) CPU
- Local Ports: 80 and 443
- Allow ping (see Devices Latency and Outage Statistics)
- bash, curl

#### <a name="installation"></a> Installation Instructions
Run the command below on the host to install and start UNMS (it will automatically install Docker if it is not installed already). If a UNMS installation already exists, it will be overwritten, but keep all data. When the process is complete, you can access UNMS at [http://localhost/](http://localhost/). You can register your devices to UNMS using this tutorial: [[Register Devices to UNMS]].

```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh
```

#### <a name="update"></a> Manual update Instructions (optional)

Starting with UNMS version 0.7.14, we have introduced a one-click update option in the UI. When a new version is available, you will be able to install the update by clicking a button in the new version notification dialog or under Settings/Maintenance.

Run this command on the host to reinstall/update to the latest version manually:
```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh --update
```

Updating the app will preserve all configuration (custom ports, SSL certificates, etc.). If you wish to change those settings, please follow the <a href="#installation-instructions">Installation Instructions</a> and specify all settings again. 

#### <a name="ssl"></a> Managing the SSL certificate for access via HTTPS (optional)
By default, UNMS uses [Let's Encrypt](https://letsencrypt.org/) to automatically create and manage an SSL certificate for its domain name. The certificate is saved under ```/home/unms/data/cert/live```. 
Use installation script arguments `--ssl-cert-dir <DIRECTORY>`, `--ssl-cert <FILENAME>`, `--ssl-cert-key <FILENAME>` and optionally `--ssl-cert-ca <FILENAME>` to manage the certificate manually.

```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh 
$ sudo bash /tmp/unms_install.sh --ssl-cert-dir /etc/certificates --ssl-cert fullchain.pem --ssl-cert-key privkey.pem
```

Make sure that the UNMS has read permission on the certificate directory and all files.

#### <a name="ports"></a> Changing the HTTP and HTTPS ports (optional)
Use installation script arguments `--http-port <NUMBER>` and `--https-port <NUMBER>` to configure the UNMS server to listen on non-standard ports. Defaults are 80 (HTTP) and 443 (HTTPS).

```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh 
$ sudo bash /tmp/unms_install.sh --http-port 8080 --https-port 8443
```

Please be aware that UNMS must be accessible from the internet via HTTP port 80 if you want to use [automatic SSL certificate management](#ssl) via Let's Encrypt.

#### <a name="ws-port"></a> Changing the device inform port (optional)
*Inform port is the port that your devices use to connect to UNMS.*

Using a separate inform port is useful when you need to expose the port outside your private network, but don't want to expose the UNMS GUI.

Use installation script argument `--ws-port <NUMBER>` to configure the UNMS WebSocket server to use a separate port for communication with your devices. 

```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh 
$ sudo bash /tmp/unms_install.sh --ws-port 8444
```

#### <a name="rproxy"></a> Running UNMS behind a reverse proxy (optional)
Use installation script arguments `--behind-reverse-proxy`, `--public-https-port <NUMBER>` and `--public-ws-port <NUMBER>` if you plan to run UNMS behind a reverse proxy server. 
Setting `--public-https-port` is only necessary if the proxy listens for HTTPS on a different port than UNMS.
Setting `--public-ws-port` is only necessary when you use <a href="#ws-port">`--ws-port`</a> to separate the inform port form the HTTPS port.

```sh
$ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh
$ sudo bash /tmp/unms_install.sh --behind-reverse-proxy --public-https-port 443 --http-port 8080 --https-port 8443 
```

Please be aware that this puts the responsibility of managing an SSL certificate on the reverse proxy and disables the automatic certificate management via Let's Encrypt. The reverse proxy must still use HTTPS for communication with UNMS, optionally with a [custom SSL certificate](#ssl). HTTP-only comunication between UNMS and the reverse proxy is not supported.

#### Cloud
We recommend using the latest version of [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) or Amazon AMI. Here are examples of suitable cloud services:
- [AWS](https://aws.amazon.com/), EC2 instance, _t2.micro_ (1 GB RAM), Ubuntu 16.04.1 LTS (Xenial Xerus)
- [DigitalOcean](https://www.digitalocean.com), basic droplet (1 GB RAM), Ubuntu 16.04.1 LTS (Xenial Xerus)

_Note: There is a detailed tutorial for DigitalOcean: **[[UNMS on DigitalOcean]]**._

#### UNMS Data
By default, the installation script ensures that the application settings and data (logs, site images, encryption key, etc.) will be stored outside of the docker container (```/home/unms/data```). This will enable you to back up that data, and more importantly, this will enable you to perform any future UNMS upgrades without any data loss.

#### Devices Latency and Outage Statistics
By default, all connected devices to UNMS will ping the UNMS host to check for latency and if any devices are being reported as offline which will result in outage statistics being generated.
Ping must be allowed to the UNMS Host for this to properly work.

## Windows, OS X

Unfortunately, Docker does not provide full support for these systems, so we can't ensure proper operation of UNMS, smooth backups and upgrades. At this time we recommend that you install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) with the latest version of [Ubuntu 16.04.1 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/) and then follow the Linux instructions above.