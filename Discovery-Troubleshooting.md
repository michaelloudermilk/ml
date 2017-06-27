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

- Check traceroute/ping from your UNMS server to device's IP.
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
    docker network create unms_default --subnet 173.30.0.1/24
    docker-compose -p unms up -d
    ```

    UNMS also contains installation parameter see [Installation Guide](https://github.com/Ubiquiti-App/UNMS/wiki/Installation-&-Update#-changing-the-unms-containers-ip-address-optional)

- The device might not be supported by UNMS

    UNMS currently (2017-06-27, version 0.8.0) supports:

    * **EdgeRouter (e50, e100, e200)**
    * **EdgeRouter (e1000)**
    * **uFiber OLT (e600)**
    * **airMAX (AC)**
    * **airMAX (M)**

## I can discover the device but connection to UNMS is failing

- Check traceroute/ping from device to your UNMS server IP / domain name.
    ```
    ping unms-server.com
    traceroute -n 192.168.x.x
    ```

- Make sure device has correct time and date set

    SSL connection requires valid time and date. This might be an issue for devices not connected to the internet or with NTP service which is disabled.

- If the device has been connected previously try refreshing it's UNMS key

    Click **_Refresh_** on the device row in devices table.

- AirMax/AirCube devices are connected through https port 443

    Currently (2017-06-27, version 0.8.0) there is no option to change the port. It will be added in the next release.

## Where to find logs on the device

Different devices store logs differently

* **EdgeRouter**, **uFiber OLT**

    ```.sh
    less /var/log/unms.error
    less /var/log/unms.log
    ```

* **airMAX**

    ```.sh
    less /var/log/messages
    ```

## Where to find UNMS logs

Logs are stored in `/home/unms/data/logs/` or you can download logs from Web UI from **Settings** -> **Maintenance** -> **Download support info**