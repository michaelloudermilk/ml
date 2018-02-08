This guide explains how to back up UNMS manually or how to migrate UNMS to another machine.

UNMS data is stored in ```/home/unms/data``` on the Docker host - settings, logs, statistics, images, backups, and SSL certificates.

### Database Backup - Settings, Logs, and Statistics
Note that backing up/migrating the databases can be done from within the UNMS app; this may be easier for most users. See Settings > Maintenance > Backup. Using this tool you can back up and restore the database instead of moving the ```/home/unms/data``` folder.

### How to Back Up the UNMS Data Folder
For data backup, you must first pause the running containers. Go to the directory, where your docker-compose.yml is located (probably `/home/unms`). Then archive the data and save it somewhere safe. As the final step, unpause the containers.

    # go to your UNMS home directory
    cd /home/unms

    #pause running containers
    sudo ~unms/app/unms-cli stop

    # pack the data directory
    sudo tar -cvjSf unms-data.tar.bz2 data

    # unpause running containers
    sudo ~unms/app/unms-cli start


This set of commands will create an archive for all UNMS settings and data. Then you can move it to another machine or archive.