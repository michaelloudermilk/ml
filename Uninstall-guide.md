This command will uninstall the UNMS docker image and containers.

Run this command as user with enabled `sudo`, in the location of `docker-compose.yml` file (probably ```/home/unms```).

    curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/uninstall.sh | sudo sh

Note: This command will not remove your settings or data. If you want to remove it too, go to ```/home/unms``` and delete the ```data``` directory.