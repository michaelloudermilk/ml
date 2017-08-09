These commands will rebuild all UNMS docker containers. Run as a user with `sudo` enabled.

    sudo docker-compose -p unms -f ~unms/app/docker-compose.yml down
    sudo docker-compose -p unms -f ~unms/app/docker-compose.yml up -d

_Note: This command will not remove your settings or data, but will log all users out of UNMS._