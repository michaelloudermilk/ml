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

If you cannot update UNMS to version 0.11.1 or higher use the following command to manually fix expired Let's Encrypt certificate. UNMS will be restarted.

```
curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/scripts/0.11.0-fix-expired-le-certificate.sh | sudo bash
```