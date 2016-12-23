This tutorial will explain how to deploy UNMS on a cloud server in a few simple steps. This tutorial uses DigitalOcean with estimated costs starting at $10/month. The usage is charged at an hourly rate, so if you use DigitalOcean to test it just for an hour, the user will only be charged $0.015. Any other cloud server of the user's choice may be used as well.

## Step 1.
**Create an account** on [DigitalOcean](https://www.digitalocean.com/).
## Step 2.
Click **Create a New Droplet**.
## Step 3.
Choose **Ubuntu 16.04.1** as the image.
![Step1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step1.png)
## Step 4.
**Choose your plan.** The $10/mo size is recommended. You can always change your plan in the future if you realize the server load is too high. You will be charged at an hourly rate, so you can try any plan for a while without incurring any high costs.
![Step2](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step2.png)
## Step 5.
**Choose a datacenter** close to your location.
![Step3](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step3.png)
## Step 6.
Check **User data** in the _Select additional options_ section.
![Step4](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step4.png)
## Step7.
Insert the following commands in the text box shown in the image above.
```
#cloud-config
runcmd:
- curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh
- sudo bash /tmp/unms_install.sh > /tmp/unms_install_output
```    
## Step 8.
Optionally, you can **add your SSH key**. This will allow you to connect to the server without using a password. Otherwise, your root password will be sent to the email associated with your DigitalOcean account.
## Step 9.
Click **Create**. 
## Step 10.
Get a coffee and wait for about 30 seconds for the server to start up, and then wait another 4-8 minutes to download and deploy UNMS on the server. You should see this:
![Step5](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step5.png)
## Step 11.
Then go to _http://your_server_ip_ (_"your_server_ip"_ is the IP of your Droplet).