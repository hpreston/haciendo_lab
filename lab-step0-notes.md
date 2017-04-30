# Lab Step 0

Starting and testing the Monolithic Application

* Web Service running in the VM
* API Service running in the VM
* SMS Service running in the VM

# Starting the Haciendo All-In-One Application 

* Log into GitHub and create a Fork of the [Haciendo Repo](https://github.com/hpreston/haciendo_aio).  

![](README_FILES/fork.jpg)

* Clone your forked repo locally to your workstation.    

```bash 
git clone http://github.com/<YOUR ACCOUNT>/haciendo_aio 
cd haciendo_aio
```

* Make a copy of the Sample Vagrantfile to use.  

```bash
cp Vagrantfile.sample Vagrantfile
```

* Open Vagrantfile and add your own Tropo Username and Password to the command that starts the SMS service.  Around line 103 in the Vagrantfile.  
  * **NOTE: DO NOT ADD THEM TO `Vagrantfile.sample`**
  * *Optional:  Change the `--tropoprefix XXXX` value to a different area code.  But note that not all area codes are supported.  Check Tropo dev docs for details.*

```
# Start sms service, log output to sms_log.log
    # *** Update the following line with your Tropo User and Password.
    nohup python haciendo_sms.py -p 5001 -t TROPO_USER -w TROPO_PASS --tropoprefix 1419 --tropourl ${SMS_NGROK_ADDRESS} > sms_log.log 2>&1 &
```

* Configure Vagrant to install Virtual Box Additions at boot.  This is needed to keep the application directory synced between your local workstation and the VM that is running the application.  

```bash
vagrant plugin install vagrant-vbguest
```

* `vagrant up` and start the Application Dev Instance.  Partial output provided below for reference.  
    * This step can take 3-5 minutes to complete.  
    * *If you do not have a local copy of the centos/7 box already, this step will automatically download one. This can take several minutes depending on Internet connection speeds.*
        * Pre-download the box: `vagrant box add centos/7 --provider virtualbox`  

```bash
# Output
[ Vagrant start and provisioning output omitted ]
.
.
.
[ When completed you will see this information ]

==> default: *******************************************************
==> default: Haciendo Application Started
==> default:  
==> default: Application services running: 
==> default: [1]   Running                 ./ngrok http 5001 &  (wd: /home/vagrant)
==> default: [2]   Running                 nohup python haciendo_sms.py -p 5001 -t TROPO_USER -w TROPO_PASS --tropoprefix 1419 --tropourl ${SMS_NGROK_ADDRESS} > sms_log.log 2>&1 &  (wd: /vagrant/sms)
==> default: [3]-  Running                 nohup python haciendo_api.py -p 5000 -t http://localhost:5001/score > api_log.log 2>&1 &  (wd: /vagrant/api)
==> default: [4]+  Running                 nohup python haciendo_web.py -p 5080 -a http://localhost:5000/api/score > web_log.log 2>&1 &
==> default:  
==> default: Browse to http://localhost:15080 to view web interface
==> default: API running on http://localhost:15000
==> default: SMS running on http://localhost:15001 
==> default:     and mapped to ngrok tunnel http://7000a93ea.ngrok.io
==> default: *******************************************************
```

* You should now be able to browse to `http://localhost:15080` and view the Web Interface.  

![](README_FILES/haciendo_web.jpg)

* You can open the following files to view the real-time logs of each service.  *If a file is missing then the application didn't start correctly.*  
    * `sms/sms_log.log`
    * `api/api_log.log`
    * `web/web_log.log`

* Enter an English phrase into the form and your own phone number to send the translation via SMS.  
    * **Remember: You are responsible for any SMS messages sent from your Tropo Account.**

# Developer Notes

* Because of the way that ngrok and the SMS service interact to provide the Tropo capabilities, you can't simply `vagrant suspend` and `vagrant resume` and expect the application to restore completely.  The recommended steps to stop and restart are: 

```bash
# Stop the vagrant box
vagrant halt

# Re-Start the vagrant box and start the app
vagrant up --provision 

# You can also restart the running box to clear any issues
vagrant reload --provision 
```