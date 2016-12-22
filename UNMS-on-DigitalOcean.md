This tutorial will demonstrate how to deploy UNMS on a cloud server in few simple steps. This tutorial uses DigitalOcean with estimated costs from $10/month. The usage is charged hourly, so if used to test just for an hour, the user will only be charged $0.015. Any other cloud server of the reader's choice may be used as well.

## Step 1.
**Create an account** on [DigitalOcean](https://www.digitalocean.com/)
## Step 2.
Click **Create a New Droplet**
## Step 3.
Choose **Ubuntu 16.04.1** as the image.
![Step1](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step1.png)
## Step 4.
**Choose your plan.** The $10/mo size is recommended. You can always change your plan in the future if you realize the server load is too high. You will be charged per hour, so you can try any plan for a while without any high costs.
![Step2](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step2.png)
## Step 5.
**Choose a datacenter** close to your location.
![Step3](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step3.png)
## Step 6.
Check "User data" in the **Select additional options** section.
![Step4](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step4.png)
## Step7.
Insert the following commands in the textbox shown in the image above.
```
#cloud-config
runcmd:
- curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh
- sudo bash /tmp/unms_install.sh > /tmp/unms_install_output
```    
## Step 8.
Optionally, you can **add your SSH key**. This will enable you to connect to the server without using a password. Otherwise, your root password will be sent to the email associated to your DigitalOcean account.
## Step 9.
Click **Create**. 
## Step 10.
Get a coffee and wait for about 30 seconds for the server to startup, and then another 4-8 mins to download and deploy UNMS on the server. You should see this:
![Step5](https://github.com/Ubiquiti-App/UNMS/blob/master/doc/DigitalOcean/step5.png)
## Step 11.
Then go to _http://your_server_ip_ where _"your_server_ip"_ is the IP of your Droplet.