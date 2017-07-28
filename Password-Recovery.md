UNMS includes a script that can be used to change an existing user's password in case password recovery be e-mail is unavailable.

To see the list of existing users in UNMS, run the the following command on the UNMS host machine:

`sudo docker exec -ti unms ./setpwd.sh`

To change a user's password, specify the username as a parameter and enter a new password when asked:

`sudo docker exec -ti unms ./setpwd.sh <username>`

NOTE: Changing the password this way turns off two-factor authentication. You can turn it on again after logging in.

