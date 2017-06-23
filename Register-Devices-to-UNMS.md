## Supported Firmware Versions

* **EdgeRouter (e50, e100, e200)**: _v1.9.1.1-unms_ or _1.9.7+_
* **EdgeRouter (e1000)**: _1.9.5_ or _1.9.7+_
* **uFiber OLT (e600)**: _1.0.0+_
* **airMAX (AC)**: _8.3.0+_
* **airMAX (M)**: _6.0.6+_ (alpha)

## Registration with UNMS Discovery
![Step0](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/discovery.png)
* Fill in subnet or IP addresses of your devices
* Click the **START** button
* Fill in your credentials
* Click **CONNECT** button

## Manual registration via device UI
This is necessary only if your UNMS is not in the same network as your devices and you can't find them with UNMS Discovery.

### Step 1
![Step1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/adddevice.png)
* Open UNMS and go to the _Devices_ section.
* Click the **ADD DEVICE** button.

### Step 2
![Step2](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/unmskey.png)
* Copy the UNMS key (it is the same UNMS key for all devices).

### Step 3
![Step3-1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/edgerouter.png)
![Step3-1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/airmax6.png)
![Step3-1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/airmax8.png)
* Open your device's administration page.
* Go to the _System_ (EdgeRouter) or _Services_ (airMAX) section.
* Paste the UNMS key.
* Enable the UNMS connection.
* Save the device configuration.

### Step 4
* Authorize your device in UNMS devices list.