Here is an example of UNMS key
wss://your.domain.com:443+n9yU137QSwTzBXnF...9Sk0pC7sDKGnpbxiHRI9W+

It consists of several parts

| Key part  | Purpose |
| ------------- | ------------- |
| wss://  | [WebSocket Secure connection protocol](https://en.wikipedia.org/wiki/WebSocket) |
| your.domain.com  | Hostname or IP of the server where UNMS runs  |
| :443 | [Port for devices to access UNMS server](https://github.com/Ubiquiti-App/UNMS/wiki/Installation-&-Update#-changing-the-http-and-https-ports-optional) |
| n9yU137QSwTzBXnF...9Sk0pC7sDKGnpbxiHRI9W | Advanced Encryption Standard key (AES key) |

What is the purpose of UNMS key?
-  To tell a device where to look for UNMS server.
-  To provide a secure communication through AES encryption

How the UNMS key works?
When a new instance of UNMS is installed, it creates its own UNMS key which is called **The Generic UNMS key**. This key represents a pointer for any device, you want to put into the system for the first time. When the Generic UNMS key is entered into a device’s settings, that device will try to connect to UNMS using the hostname / IP and a port parts of that key.

If the connection is successful the AES key part of UNMS key is used for secure communication between the device and UNMS. When the connection is established or the first time then a new AES key is generated for the device. This new AES key is replace the original AES key in the generic UNMS key and by doing that it creates **The Device Specific UNMS key**.

Then the device specific UNMS key rewrites the generic UNMS key on the device and UNMS stores device’s MAC address and AES key in PostgreSQL database. From now on, each time the device wants to communicate with UNMS, AES key part of device specific UNMS key is used and UNMS uses AES key from PostgreSQL database for decryption/encryption. 

