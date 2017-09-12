# Problem with AES encryption

Typical example:

    2017-07-26 11:53:04 ERROR failed to decrypt AES-encrypted message
    2017-07-26 11:53:04 ERROR failed to decode message from UNMS

**Important notice: There is an important bug-fix in UNMS 0.9.2 which decreases occurrence of this issue.**

This error means that AES key in your device and in UNMS for this device mismatch. There could be several reasons. Longer but the most reliable solution:
1. disable UNMS and remove UNMS key in your device UI
2. click refresh button on your device in UNMS device list
3. if you can’t see your device in UNMS then you should remove device MAC from Postgres (this is very rare)
4. click add device button in UNMS and copy UNMS key to clipboard
5. paste UNMS key to your device and enable UNMS
6. your device should be connected back in UNMS

Shorter solutions with possible disadvantages:
* Remove device from UNMS but you will lost device statistics, backups etc.
* Use UMobile discovery or UNMS Discovery which fix AES key when you connect your device with it but they have to have access to your device. It's not possible for example with cloud UNMS.

### How to remove device MAC address from Postgres:
You will find MAC address only for devices with V2 communication protocol ( EdgeRouter 1.9.7.betaX+, airMAX 8.4+ ). You can list all MAC addresses or skip this step if you know it:
```
sudo docker exec -u postgres -it unms-postgres psql
\c unms
select * from mac_aes_key;
\q
```

1. Stop UNMS to ensure we clear it from memory as well.
2. Choose you device MAC address and replace it in following script:

```
sudo docker exec -u postgres -it unms-postgres psql
\c unms
delete from mac_aes_key where mac='44:d9:e7:50:92:89';
\q
```
### How UNMS keys aka UNMS connection strings works

Devices with communication protocol V1 (EdgeRouter 1.9.1.1-unms up to 1.9.7alphaX, airMAX 8.3.0):
1. Device connects to UNMS with universal AES key
2. UNMS verify that key and messages are encoded with universal AES key and accepts connection
3. User authorise device and UNMS has full access to device

Devices with communication protocol V2 (EdgeRouter 1.9.7.betaX+, airMAX 8.4+):
1. Device connects to UNMS with universal AES key
2. UNMS generate device specific AES key and sends it back to device
3. Device updates its UNMS key with unique AES key and reconnects back to UNMS
4. UNMS accepts this connection and locks communication with this device only to unique UNMS key
3. User authorises device and UNMS has full access to device

# Connection error

Typical example:

     Sep 12 15:20:52 udapi-bridge[978]: connecting to XXX.YYY.ZZZ:443
     Sep 12 15:20:57 udapi-bridge[978]: connection error (XXX.YYY.ZZZ:443)

This is a generic network error which means that your device can’t reach UNMS server. It is necessary to check that your device can ping your UNMS server and it can connect to UNMS via websocket with SSL. There is a detailed tutorial how to check it on this [wiki page](https://github.com/Ubiquiti-App/UNMS/wiki/Discovery-Troubleshooting#i-can-discover-the-device-but-connection-to-unms-is-failing). If your device is EdgeRouter and you are using it as a gateway then you could have a problem with resolving UNMS hostname and you have to use IP address in your UNMS key.

# Connection terminated by UNMS (establishing connection)

Typical example:

     Sep  9 14:25:16 udapi-bridge[3300]: peer closed connection (status 1000): Connection terminated by UNMS while establishing connection.                                                                                      
     Sep  9 14:25:16 udapi-bridge[3300]: connection closed  

These log lines mean that device can reach UNMS but UNMS terminates this connection. It's typically followed by this error in UNMS log:

     [log,error,ws] data: WS - problem with establishing connection from <ip of site>, unexpected data: Error: Unsupported state or unable to authenticate data)

It means that device has wrong UNMS key (more precisely AES key) or there could be a problem with data parsing. This problem has the same solution as [Problem with AES encryption](https://github.com/Ubiquiti-App/UNMS/wiki/Device-log-Troubleshooting#problem-with-aes-encryption). There is more info about it in this [community post](https://community.ubnt.com/t5/UNMS-Ubiquiti-Network-Management/EdgeRouter-Pro-fails-to-show-in-console/m-p/2053737#M1001). This problem could have multiple reasons but the most probable one is that something replaced device specific UNMS key with universal UNMS key. There is a bug ([beta community post](https://community.ubnt.com/t5/airCube-ISP-AC-Beta/Firmware-1-0-2-Web-UI-Issue/m-p/2056059#M530)) in UMobile application which could replace device specific UNMS key with universal UNMS key.  

# Connection terminated by UNMS (system info)

Typical example:

    Jul 28 22:26:41 dapi-bridge[459]: peer closed connection (status 1000): Connection terminated by UNMS while getting system info.

This log line means that UNMS can't connect this device. It could mean that its model or FW is not supported or there is a problem with parsing device info. If it happens, please contact us via [UNMS community](https://community.ubnt.com/t5/UNMS-Ubiquiti-Network-Management/bd-p/UNMSBeta). We would like to know your device model, FW version and [UNMS logs](https://github.com/Ubiquiti-App/UNMS/wiki/Discovery-Troubleshooting#where-to-find-unms-logs) and output of this command:

     ls /sys/class/net


# Problem with a custom reverse proxy

Typical example:

     Jul 29 11:24:11 udapi-bridge[790]: connection error (localhost:443): HS: ACCEPT missing
    
Devices use [WebSocket Secure connection](https://en.wikipedia.org/wiki/WebSocket) aka WSS for communication with UNMS. Therefore you have to configure your proxy to handle websocket communication with TLS properly on its public-https-port. See our [example configurations](https://github.com/Ubiquiti-App/UNMS/wiki/Reverse-proxy-examples) for Nginx and Apache.

# UNMS connector can't access local device data

Typical example:

    2017-08-06 00:37:55 ERROR recv(): socket closed
    2017-08-06 00:37:55 ERROR failed to parse header
    2017-08-06 00:37:55 ERROR failed to perform EdgeOS socket request
    2017-08-06 00:37:55 ERROR connect(): Connection refused
    2017-08-06 00:37:55 ERROR failed to initialize stats socket
    2017-08-06 00:37:57 ERROR connect(): Connection refused
    2017-08-06 00:37:57 ERROR failed to initialize stats socket

These error log lines mean that UNMS connector (udapi-bridge) can't access local device sockets and receive device statistics and read configuration. In this case, there should be more information in log file _/var/log/ubnt-daemon.log_ Plus it's possible to get more information from udapi-bridge which is in verbose mode as well. 

### Howto switch udapi-bridge to verbose mode.
Steps:
1. Disable UNMS connection in device UI 
2. Copy UNMS key
3. Connect to device via SSH
4. Run udapi-bridge manually and check its output.

Command:

    sudo udapi-bridge -v UNMS-KEY