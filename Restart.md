This command will restart all UNMS docker containers. Run this command as user with `sudo` enabled.

    sudo docker restart $(docker ps -a -q --filter="name=unms")

_Note: This command will not remove your settings or data, but will log all users out of UNMS._