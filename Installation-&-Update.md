Welcome to the UNMS installation & update guide. UNMS can be deployed as a docker image on your own server. Please, choose your system and follow the instructions to install a fresh copy or update an existing installation.

## Linux

Run this command to install docker, pull the latest UNMS image and start it. If an UNMS installation already exists, it will be updated to the latest version.

    curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh && sudo bash /tmp/unms_install.sh

_Note: By default, the installation script ensures that the application settings and data (logs, pictures, encryption key, etc.) will be stored outside the docker container (```/home/unms/data```). This will enable you to backup that data and more importantly, this will enable you to perform any future UNMS upgrades without any data loss._

When done the UNMS can be found at [http://localhost/](http://localhost/).

## Windows, OS X

Unfortunately, Docker does not provide full support for these systems. Thus we can't ensure smooth backups and upgrades. At this moment we recommend to install VirtualBox with latest version of Ubuntu and then follow the Ubuntu instructions above.