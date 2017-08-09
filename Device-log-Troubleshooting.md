# Problem with AES encryption

Typical example:

    2017-07-26 11:53:04 ERROR failed to decrypt AES-encrypted message
    2017-07-26 11:53:04 ERROR failed to decode message from UNMS

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

    `sudo docker exec -u postgres -it unms-postgres psql`
    `\c unms`
    `select * from mac_aes_key;`
    `\q`

1. Stop UNMS to ensure we clear it from memory as well.

2. Choose you device MAC address and replace it in following script:

    `sudo docker exec -u postgres -it unms-postgres psql`
    `\c unms`
    `delete from mac_aes_key where mac='44:d9:e7:50:92:89';`
    `\q`

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

# Problem with a custom reverse proxy

Typical example:

     Jul 29 11:24:11 udapi-bridge[790]: connection error (localhost:443): HS: ACCEPT missing
    
Devices use [WebSocket Secure connection](https://en.wikipedia.org/wiki/WebSocket) aka WSS for communication with UNMS. Therefore you have to configure your proxy to handle websocket communication with TLS properly on its public-https-port.

# UNMS terminates device connection

Typical example:

    Jul 28 22:26:41 dapi-bridge[459]: peer closed connection (status 1000): Connection terminated by UNMS while getting system info.

This log lines means that UNMS can't connect this device. It could mean that its model or FW is not supported or there is a problem with parsing device info. If it happens, please contact us via [UNMS community](https://community.ubnt.com/t5/UNMS-Ubiquiti-Network-Management/bd-p/UNMSBeta). We would like to know your device mode, FW version and [UNMS logs](https://github.com/Ubiquiti-App/UNMS/wiki/Discovery-Troubleshooting#where-to-find-unms-logs).