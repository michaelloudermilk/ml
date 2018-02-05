## Restart (0.10.0+)

Use the following command to rebuild all UNMS docker containers. Run as a user with `sudo` enabled.

    sudo ~unms/app/unms-cli restart

_Note: This command will not remove your settings or data, but will log all users out of UNMS._

## Stop (0.10.0+)

This command stop UNMS containers. Run as a user with `sudo` enabled.

    sudo ~unms/app/unms-cli stop

## Start (0.10.0+)

This command start UNMS containers. Run as a user with `sudo` enabled.

    sudo ~unms/app/unms-cli start

## Password recovery (0.10.0+)

UNMS includes a script that can be used to change an existing user's password in case password recovery be e-mail is unavailable. To see the list of existing users in UNMS, run the the following command on the UNMS host machine:

    sudo docker exec -ti unms ./setpwd.sh

To change a user's password, specify the username as a parameter and enter a new password when asked:

    sudo docker exec -ti unms ./setpwd.sh <username>

_Note: Changing the password this way turns off two-factor authentication. You can turn it on again after logging in._

## Refresh Let's Encrypt certificate (0.12.0+)

This command refresh Let's Encrypt certificte. Run as a user with `sudo` enabled.

    sudo ~unms/app/unms-cli refresh-certificate


## Fix redis aof file (0.12.0+)

This command fix corrupted redis aof file. Run as a user with `sudo` enabled.

    sudo ~unms/app/unms-cli fix-redis-aof