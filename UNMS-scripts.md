## Generating support files via CLI

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

You can find the `<device-id>` in the URL in UNMS:

[[/images/deviceid.png|finding device ID]]



