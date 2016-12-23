This guide explains how to back up UNMS manually or how to migrate UNMS to another machine.

UNMS data is stored in ```/home/unms/data``` - settings, logs, statistics, images, backups & SSL certificates.

### Database backup - settings, logs & statistics
Note that backing up/migrating the databases can be done from within the UNMS app; this may be easier for most users. See Settings > Maintenance > Backup. Using this tool you can back up and restore the database instead of moving the ```/home/unms/data``` folder.

### How to back up the UNMS data folder
For data backup, you must first pause the running containers. Go to directory, where your docker-compose.yml is located (probably `/home/unms`). Then archive the data and save it somewhere safe. Finally unpause the containers.

    # go to your UNMS home directory
    cd /home/unms

    #pause running containers
    docker-compose pause

    # pack the data directory
    sudo tar -cvjSf unms-data.tar.bz2 data

    # unpause running containers
    docker-compose unpause

This set of commands will create an archive for all UNMS settings and data. Then you can move it to another machine or archive.