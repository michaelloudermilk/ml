## Generating support files

Use the following script to generate a .tar.gz file with the latest logs and all info that will help us troubleshoot issues with your UNMS installation.

```
curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/scripts/get-support-info.sh | sudo bash -s -- --restart
```

The `--restart` parameter is optional and tells the script to start by rebuilding all UNMS containers. This will ensure that the support file includes any errors generated during startup.

## Exporting device statistics

Use the following script to generate a .tar.gz file with all statistics associated with a specific device:

```
curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/scripts/get-statistics.sh | sudo bash -s -- <device-id>
```

You can find the `<device-id>` in the URL in UNMS.


## Workaround for expired Let's Encrypt certificates in UNMS 0.11.0

If you cannot update UNMS to version 0.11.1 or higher use the following commands to manually fix expired Let's Encrypt certificate. These commands need to be run with root privileges.

```
cd /home/unms/data/cert # Change this line if you installed UNMS to a different location.
backupDir="le-node.bak"
if [ -e "./accounts" ] ; then mv -f "./accounts" "${backupDir}"; fi
if [ -e "./archive" ] ; then mv -f "./archive" "${backupDir}"; fi
if [ -e "./live" ] ; then mv -f "./live" "${backupDir}"; fi
if [ -e "./renewal" ] ; then mv -f "./renewal" "${backupDir}"; fi
latestCertDir="$(find "./le-node.bak/live" -name "privkey.pem" -exec ls -tR {} + | head -1 | xargs dirname)"
ln -fs "${latestCertDir}/privkey.pem" "live.key"
ln -fs "${latestCertDir}/fullchain.pem" "live.crt"
docker-compose -p unms -f ../../app/docker-compose.yml restart
```
