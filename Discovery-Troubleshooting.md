# Overview

- [How The Discovery Works](#how-the-discovery-works)
- [I can't see any devices or some devices are missing](#i-cant-see-any-devices-or-some-devices-are-missing)
- [I can discover the device but connection to UNMS is failing](#i-can-discover-the-device-but-connection-to-unms-is-failing)
- [Where to find logs on the device](#where-to-find-logs-on-the-device)
- [Where to find UNMS logs](#where-to-find-unms-logs)

## How The Discovery Works

UNMS sends _discovery packet_ to every IP address in the given range and waits for device reply. The device sends back basic information shown in the Discovery Manager - name, SSID, model, firmware version, mac address and list of all interfaces.

The discovery packet is sent using UDP to port 10001, it consists of four bytes `0x1 0x0 0x0 0x0`.

## I can't see any devices or some devices are missing

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

- The device might not be supported by UNMS

    UNMS currently (2017-06-27, version 0.8.0) supports:

    * **EdgeRouter (e50, e100, e200) - FW 1.9.1.1-unms, FW 1.9.7+**
    * **EdgeRouter (e1000) - FW 1.9.5+**
    * **uFiber OLT (e600) - FW 1.0.0+**
    * **airMAX (AC) - FW 8.3+**
    * **airMAX (M) - FW 6.0.6-alpha-unms, FW 6.0.7+**

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

    Click **_Refresh_** on the device row in devices table.

- AirMax/AirCube devices are connected through https port 443

    Currently (2017-06-27, version 0.8.0) there is no option to change the port. It will be added in the next release.

## Where to find logs on the device

Different devices store logs differently. You have to log to your device via SSH and then:

### **EdgeRouter** and **uFiber OLT**

    less /var/log/unms.error
    less /var/log/unms.log
 
### **EdgeSwitch**

Enter into [user privilege mode](https://dl.ubnt.com/guides/edgemax/EdgeSwitch_CLI_Command_Reference_UG.pdf).

    enable

Find UNMS info in a support info

    show tech-support system

### **airMAX**

    less /var/log/messages

### **airCube**

You can download support info via UI. It's a small blue lifebuoy icon.

## Where to find UNMS logs

Logs are stored in `/home/unms/data/logs/` or you can download logs from Web UI from **Settings** -> **Maintenance** -> **Download support info**

You can use a script to generate a package with the latest logs and additional info required to troubleshoot UNMS issues, see [[Generating a support info file via CLI]]


## Troubleshooting domain name resolution issues

In some scenarios, Docker will use only the first name server configured on the host system and not move to the next if it fails. https://github.com/moby/moby/issues/20494
Docker also defaults to 8.8.8.8 if no DNS is configured https://robinwinslow.uk/2016/06/23/fix-docker-networking-dns/