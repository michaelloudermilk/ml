# Overview

- [How The Discovery Works](#how-the-discovery-works)
- [I can't see any devices or some devices are missing](#i-cant-see-any-devices-or-some-devices-are-missing)
- [I can discover the device but connection to UNMS is failing](#i-can-discover-the-device-but-connection-to-unms-is-failing)
- [Where to find logs on the device](#where-to-find-logs-on-the-device)
- [Where to find UNMS logs](#where-to-find-unms-logs)

## How The Discovery Works

UNMS sends _discovery packet_ to every IP address in the given range and waits for device reply. The device sends back basic information shown in the Discovery Manager - _name_, _SSID_, _model_, _firmware version_, _mac address_ and list of all _interfaces_. The discovery packet is sent using UDP to port 10001, it consists of four bytes `0x1 0x0 0x0 0x0`.

## I can't see any devices or some devices are missing

- Known issues
    * There is a limited number of UDP connections which can be opened from UNMS private docker network. It's possible that thanks to it you will not see all devices in your network. We will introduce support for discovery over TCP and not only via UDP. This will improve discovery results in UNMS 0.12.0+. But it will not help in all cases because not all devices and FW versions are listening for TCP connections (for example ToughSwitches). There is a workaround for current UDP discovery with searching only small, for example 24 subnets and waiting a minute between searches.
   * It's not possible to find devices from cloud UNMS instances. Therefore UNMS 0.12.0+ will be able to search not connected devices thanks to already connected devices in UNMS. You can add one device via UNMS mobile app or add it manually and other devices will automatically pop up in UNMS thanks to this device or devices. This will help with previous issue as well.  

- Check that discovery is enabled because many devices allow to turn the discovery off: 
   * **AirMax** - check the `SETTINGS` -> `Services` -> `Device Discovery`. If the checkbox is set to `OFF` change it to `ON` and click `Save Settings`.
   * **EdgeRouters** - check the `SETTINGS` -> `System` -> ` UBNT Discovery`. If the checkbox is set to `OFF` change it to `ON` and click `Save`. Plus you need to check if there isn't an active firewall record for blocking UDP packets on port 10001. You can find a nice guide [here](https://github.com/Ubiquiti-App/UNMS/wiki/Discovery#edgerouter).

- Check traceroute/ping/curl from your UNMS server to device's IP.
    ```
    ping 192.168.x.x
    traceroute -n 192.168.x.x
    ```

- Check collision with Docker's virtual subnet

    While rare the container's IP might collide with real IP in your network. Run `ifconfig` on your UNMS server to see Docker's IPs:
    <pre>
    $ ifconfig
    br-6805a10e2e92 Link encap:Ethernet  HWaddr 02:42:d3:f8:f1:64
              inet addr:<b>172.18.0.1</b>  Bcast:0.0.0.0  Mask:255.255.0.0
              inet6 addr: fe80::42:d3ff:fef8:f164/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:196618 errors:0 dropped:0 overruns:0 frame:0
              TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:5506244 (5.5 MB)  TX bytes:764 (764.0 B)
    
    docker0   Link encap:Ethernet  HWaddr 02:42:dd:1c:c3:57
              inet addr:<b>172.17.0.1</b>  Bcast:0.0.0.0  Mask:255.255.0.0
              inet6 addr: fe80::42:ddff:fe1c:c357/64 Scope:Link
              UP BROADCAST MULTICAST  MTU:1500  Metric:1
              RX packets:8 errors:0 dropped:0 overruns:0 frame:0
              TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:536 (536.0 B)  TX bytes:648 (648.0 B)
    </pre>
    If such a IP exists in your network you will have to change Docker's subnet

    ```
    cd /home/unms/app
    docker-compose -p unms down
    docker network create unms_default --subnet 172.30.0.1/24
    docker-compose -p unms up -d
    ```

    UNMS also contains installation parameter see [Installation Guide](https://github.com/Ubiquiti-App/UNMS/wiki/Installation-&-Update#-changing-the-unms-containers-ip-address-optional)

- Check with tcpdump that your device receives UDP packet from your UNMS instance and sends back an answer.

- Only 24 ranges are allowed for public IP addresses

- The device might not be supported by UNMS. Discovery should be able to find following devices (all FW versions):

    UNMS 0.11.1+ supports:
    * EdgeRouter - device will be updated at least to FW version 1.9.7-hotfix4 during connection via discovery
    * EdgeSwitch - UNMS discovery can update only FW 1.4.0+, it will be updated to 1.7.3
    * uFiber OLT - any FW is supported by UNMS
    * airMAX (AC/M) - device will be updated at least to FW version 6.1.3 for airMAX M and 8.4.3 for airMAX AC
    * AirCube - any FW is supported by UNMS

   UNMS 0.12.x+ will support:
    * airFiber - it will not be possible to connect all of them in 0.12.0 to UNMS
    * ToughSwitch - device will be updated to FW 1.4.0

## I can discover the device but connection to UNMS is failing

- Check traceroute/ping/curl from device to your UNMS server IP / domain name.
    ```
    ping unms-server.com
    traceroute -n 192.168.x.x
    curl --insecure https://192.168.x.x:443/v2.1/nms/version
    ```

- Check HTTPS upgrade to WebSocket
    ```
    curl --insecure --include --no-buffer --header "Connection: Upgrade" --header "Upgrade: websocket" --header "Host: example.com:80" --header "Origin: http://example.com:80" --header "Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==" --header "Sec-WebSocket-Version: 13" https://192.168.x.x:443/
    ```
  The values of the `--header` parameters do not matter, you can use example.com as shown. Only the last parameter must be the real address of your UNMS server.

  This should return output similar to the following, indicating that upgrade to WebSocket was successful:
    ```
    HTTP/1.1 101 Switching Protocols
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Accept: qGHgK3En71di5rrssAZTmtRTyFk=
  
  db332780afad264425162cb11d19ffd1ac9ad3658f63cb56968a8a2592054a39aac950bcdae301e39eea75f28c7d3e7dd32a568f0fcf67f25b692434c825ffc7d13b7f8bcec1fb649919d784723f039ef50deb939eeb2b1bd602f56339ac20b65b3
    ```


- Make sure device has correct time and date set

    SSL connection requires valid time and date. This might be an issue for devices not connected to the internet or with NTP service which is disabled.

- If the device has been connected previously try refreshing it's UNMS key

    Click **_Refresh_** on the device row in devices table. If it doesn't help you should manually paste universal UNMS key via device UI.

## Where to find logs on the device

Different devices store logs differently. The first option is to download support file which includes logs via device UI or you have to log to your device via SSH and then:

### **EdgeRouter** and **uFiber OLT**

    less /var/log/unms.error
    less /var/log/unms.log
 
### **EdgeSwitch**

Enter into [user privilege mode](https://dl.ubnt.com/guides/edgemax/EdgeSwitch_CLI_Command_Reference_UG.pdf).

    enable

Find UNMS info in a support info

    show unms log

### **airMAX**
Device service syslog has to be allowed.

    less /var/log/messages

Or file syslog.txt from device support file.

### **airCube**

You can download support info via UI. It's a small blue lifebuoy icon. UNMS logs are included in FW 1.1.0+ (syslog).

## Where to find UNMS logs

Logs are stored in `/home/unms/data/logs/` or you can download logs from Web UI from **Settings** -> **Maintenance** -> **Download support info**

UNMS update log is in file `/home/unms/data/update/update.log`

## Troubleshooting domain name resolution issues

In some scenarios, Docker will use only the first name server configured on the host system and not move to the next if it fails. https://github.com/moby/moby/issues/20494
Docker also defaults to 8.8.8.8 if no DNS is configured https://robinwinslow.uk/2016/06/23/fix-docker-networking-dns/