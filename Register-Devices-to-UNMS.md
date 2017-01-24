## Supported Firmware Versions

_**Note: FW is available in [UNMS Community](https://community.ubnt.com/t5/UNMS-Next-Generation-Network/UNMS-Compatible-EdgeOS-Firmware/m-p/1810799/thread-id/21)**_

#### EdgeRouter
* ER-X/ER-X-SFP/EP-R6: Firmware v1.9.2alpha1
* ER-8/ERPro-8/EP-R8: Firmware v1.9.2alpha1
* ERLite-3/ERPoe-5: Firmware v1.9.2alpha1

#### U Fiber
* UF-OLT: Firmware v0.2.6alpha

## How to Register the EdgeRouter to UNMS

### Step 1
![Step1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/step1.png)
* Open UNMS and go to the _Devices_ section.
* Click the **ADD DEVICE** button.

### Step 2
![Step2](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/step2.png)
* Copy the UNMS connection string (it is the same UNMS connection string for all devices).

### Step 3
![Step3](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/step3.png)
* Open your device's administration page.
* Go to the _System_ section.
* Paste the UNMS connection string.
* Enable the UNMS connection.

### Step 4
![Step4](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/step4.png)
* Save the device configuration.

### Step 5
![Step5](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/register/step5.png)
* Authorize your device in UNMS.

_Note: We are working on a simpler registration process with the U Mobile application, and we will add support for the discovery service._