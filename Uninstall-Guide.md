Use the following command to stop and remove all UNMS docker containers. Run as a user with sudo enabled.

    sudo ~unms/app/unms-cli stop

_Note: This command will not remove your settings or data. If you want to remove everything, delete ```~unms/data``` and ```~unms/app```._

> ### Versions 0.9.4 and older
> 
> This command will uninstall the UNMS docker image and containers.
> 
> Run this command as user with enabled `sudo`, in the location of the `docker-compose.yml` file (probably ```/home/unms```).
> 
>     curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/uninstall.sh | sudo sh
> 
> _Note: This command will not remove your settings or data. If you want to remove your data too, then go to ```/home/unms``` and delete the ```data``` directory._