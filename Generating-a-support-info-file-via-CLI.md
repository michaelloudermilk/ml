Please use the following script to generate a .tar.gz file with the latest logs and all info that will help us troubleshoot your issues with the UNMS installation.

```
curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/scripts/get-support-info.sh | sudo bash -s -- --restart
```

The `--restart` option tells the script to start by rebuilding all UNMS containers. This will ensure that the support file includes any errors generated during startup.