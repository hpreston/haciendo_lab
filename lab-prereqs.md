# Lab Prereqs and Prep

To complete the Haciendo Lab you will need to have the following accounts, software and information available and ready to go.  

## Accounts 

* [GitHub](https://github.com) - You will need a free account with GitHub.  
* [Docker Hub](https://hub.docker.com) - You will need a free account with Docker Hub.
* [Cisco Tropo](https://tropo.com) - You will need a free account with Tropo.  
  * Your account needs to be enabled for **sending** SMS Messages.  You can request this by sending an email to [support@tropo.com](mailto:support@tropo.com).  This can take 1-2 business days so be sure to send this email in advance of completing the lab.  
* [Yandex Translate API](https://api.yandex.com/translate/) - You will need an account and free API key for the Yandex Service
    * Go to [https://api.yandex.com/translate/](https://api.yandex.com/translate/) and click to get a "Free API Key".  
    * Sign-up and copy your key to a text file to keep handy
    * Keys can be found after sign-up at [https://tech.yandex.com/keys/](https://tech.yandex.com/keys/)

## Software Installed on Workstation 

* [git](https://git-scm.com/downloads) 
* [Vagrant + Virtual Box](https://www.vagrantup.com/downloads.html)
    * Pre-download Vagrant box to save time 

        ```bash
        vagrant box add centos/7 --provider virtualbox
        ```

* [Docker for X](https://www.docker.com)
    * Download and install the appropriate Docker for your platform.  During this lab you will be building, running, and pushing docker images
    * Pre-download Docker base images used in the lab to save time

        ```bash
        docker pull python:3.5-alpine
        docker pull python:2.7-alpine
        ```

* [Home Brew](http://brew.sh)
    * Mac Only 
    * Package manager for installing Linux Applications 
* [ngrok](https://ngrok.com/)
    * You don't **need** this preinstalled as during the lab you will be instructed to download and run from a temporary directory.  However, feel free to become familiar with this handy tool.  

## Fork the Lab Application 

* Log into GitHub and create a Fork of the [Haciendo Repo](https://github.com/hpreston/haciendo_aio).  

![](README_FILES/fork.jpg)

* Clone your forked repo locally to your workstation.    

```bash 
git clone http://github.com/<YOUR ACCOUNT>/haciendo_aio 
cd haciendo_aio
```    

