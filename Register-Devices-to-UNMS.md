## Supported Firmware Versions

* **EdgeRouter**: _1.9.7+_
* **EdgeSwitch**: _1.7.3+_
* **uFiber OLT**: _1.0.0+_
* **airMAX (AC)**: _8.4.1+_
* **airMAX (M)**: _6.1.3+_ 
* **airCube**: _1.0.0+_ 

## Registration with UNMS Discovery
* Go to UNMS Discovery manager
* Fill in subnet or IP addresses of your devices
* Click the **START** button
* Fill in your credentials
* Click **CONNECT** button

## Manual registration via device UI
This is necessary only if your UNMS is not in the same network as your devices and you can't find them with UNMS Discovery.

### Step 1
* Open UNMS and go to the _Devices_ section.
* Click the **ADD DEVICE** button.

### Step 2
* Copy the UNMS key (it is the same UNMS key for all devices).

### Step 3
* Open your device's administration page.
* Go to the _System_ (EdgeRouter) or _Services_ (airMAX) section.
* Paste the UNMS key.
* Enable the UNMS connection.
* Save the device configuration.

### Step 4
* Authorize your device in UNMS devices list.