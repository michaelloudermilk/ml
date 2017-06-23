## Supported Firmware Versions

* **EdgeRouter (e50, e100, e200)**: _v1.9.1.1-unms_ or _1.9.7+_
* **EdgeRouter (e1000)**: _1.9.5_ or _1.9.7+_
* **uFiber OLT (e600)**: _1.0.0+_
* **airMAX (AC)**: _8.3.0+_
* **airMAX (M)**: _6.0.6+_ (alpha)

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