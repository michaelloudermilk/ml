UNMS includes a script that can be used to change an existing user's password in case password recovery be e-mail is unavailable.

To change a UNMS user's password, run the following command on the UNMS host machine and enter a new password when asked:

`sudo docker exec -ti unms sh -c "/home/app/unms/setpwd.sh <username>"`

NOTE: Changing the password this way turns off two-factor authentication. You can turn it on again after logging in.