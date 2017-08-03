# Issue with device connection to UNMS (AES encryption)

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

Shorter solutions with possible disadvantage:
* Remove device from UNMS but you will lost device statistics, backups etc.
* Use UMobile discovery or UNMS Discovery which fix AES key when you connect your device with it but they have to have access to your device. It's not possible for example with cloud UNMS.

### How to remove device MAC address from Postgres:
List all MAC addresses or skip this step if you know it:

    sudo docker exec -u postgres -it unms-postgres psql
    \c unms
    select * from mac_aes_key;
    \q

Choose you device MAC address and replace it in following script:

    sudo docker exec -u postgres -it unms-postgres psql
    \c unms
    delete from mac_aes_key where mac='44:d9:e7:50:92:89';
    \q
