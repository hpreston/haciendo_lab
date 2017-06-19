# Haciendo!

Haciendo is a fun little application designed to be used as part of an Application Refactoring Lab.  In the lab, you will take a "Monolithic Application" where all of the services for the application are installed and running on a single virtual machine, and "refactor" it to break each service into separate Docker images in a "microservice" architecture.  

The lab guide can be found at [Refactoring Hands-On Lab](https://github.com/hpreston/haciendo_lab).  

The application code can be found at [Haciendo Application](https://github.com/hpreston/haciendo_aio).  

# Haciendo! Lab

Refactoring Learning Lab to accompany the app [haciendo_aio](https://github.com/hpreston/haciendo_aio).  This lab has the following steps through which you will run.  

1. [Pre-Reqs](lab-prereqs.md): Should be started and completed a week prior to the day of the lab.  This is because getting your Tropo Dev Account fully setup can take a few days.  
2. [Step 0: Starting the Monolithic Application](lab-step0-notes.md)
3. [Step 1: Refactoring the Web Service](lab-step1-notes.md)
4. [Step 2: Refactoring the API Service](lab-step2-notes.md)
5. [Step 3: Refactoring the SMS Service](lab-step3-notes.md)
6. [Bonus: Deploying Haciendo to the Cloud](lab-bonus-notes.md)

# Technical Bits

Here is some basic information about the application that maybe helpful.  

## Web

In the `web/` directory you will find the Web front end for the application.  It is written in Python, leverages the Flask web framework and bootstrap styling to create a responsive and clean web interface.  

When a user submits a request in the web form, it is `POSTED` back to the web server where the user request is forward to the **API** service for processing.  

It is designed to run with Python 3.5, though may run in other versions of Python.  For convenience, a `requirements.txt` file is included in the application directory.  

After starting the application, it will be available at [http://localhost:15080](http://localhost:15080).

## API

In the `api\` directory you will find the API layer for the application.  It is written in Python and leverages Flask and Flask_Restful to create a REST API layer for the application.  

The API Service uses the free translation API provided by [Yandex](http://translate.yandex.com) to convert English -> Spanish.  This service, while free, does require an API key to use.  See the Pre-Reqs for setting up an account.  

It is designed to run with Python 3.5, though may run in other versions of Python.  For convenience, a `requirements.txt` file is included in the application directory.  

After starting, the following API is available to be used directly.  

```bash
curl -X POST localhost:15000/api/score \
  -d 'line=hello' \
  -d 'phonenumber=55555555555'
```

## SMS

In the `sms\` directory you will find the SMS Service for the application.  It is written in Python and leverages itty to create a basic REST API.  Haciendo leverages Tropo to send and recieve SMS messages to public phone numbers, and uses the tropo-webapi-python library.  

To run this application, you will need an account with Tropo that has been enabled for outbound SMS messaging.  

This service **MUST** run using Python 2.7 due to limitations of the leveraged libraries and modules.  

After starting the following APIs are available.  

* Retrieve information about the Tropo Application

```bash
curl localhost:15001/application
```
* Retrieve the phone number registered to the application 

```bash
curl localhost:15001/application/number
```

* Send a hello message from the Tropo Service

```bash
curl localhost:15001/hello/5555555555
```

## Developer Tooling 

To run and develop this application, some key tools are being leveraged.  

[Vagrant](http://vagrantup.com), along with Virtual Box is being used to quickly enable developers to startup the full application simply with `vagrant up`.  A sample Vagrantfile is provided in the repo that only needs to have your Tropo Credentials inserted to get started.  You will need to install Vagrant onto your local workstation before starting.  Vagrant should install Virtual Box automatically.  

[ngrok](https://ngrok.com/) is being used to expose the Tropo Service running within the Vagrant VM to the internet so that API calls can be successfully sent from the Tropo Cloud to the application.  The installation of ngrok into the Vagrant managed VM is done automatically, and no special account is needed to use this tool.  

[Docker](http://docker.com) will be used during the lab exercise to build and execute containers for each of the services of Haciendo.  You should install Docker on your workstation before beginning the lab.  

Though you can complete this lab with any IDE of your choice, [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/) was used in the development and testing of the application.  




