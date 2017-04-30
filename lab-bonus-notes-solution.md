# Lab Bonus

Deploy Refactored Haciendo to the Cloud!  

* Publish the web, api, and sms containers to Docker Hub 
* Build Marathon Application Templates for each service 
* Deploy to DEVNET Mantl Sandbox 
* Uninstall the Application from the Mantl Sandbox

This bonus step isn't spoon fed to you on a silver platter.  The information needed to complete is provided, along with hints.  However, to be successful you will need to put a little work into it!

## Prerequisites and Info 

* [hub.docker.com](https://hub.docker.com) Account (free is OK)
* [DEVNET Mantl Sandbox Access](https://devnetsandbox.cisco.com/RM/Diagram/Index/94487c25-2b16-4204-b45f-09a5939b0f57?diagramType=Topology)
    * Creds: admin/1vtG@lw@y
    * Address [https://mantlsandbox.cisco.com](https://mantlsandbox.cisco.com)
        * Marathon API Running on: https://mantlsandbox.cisco.com:8080
    * Application Domain: "app.mantldevnetsandbox.com"
        * Applications deployed to the DEVNET Sandbox will be publicly available at "http://APP-ID.app.mantldevnetsandbox.com"
        * For URLs, "/"s are replaced with "-".  Therefore an App ID of "/jsmith/haciendo/web" would create a url of "http://jsmith-haciendo-web.app.mantldevenetsandbox.com"

## Reference Sites

* [Marathon REST API Guide](https://mesosphere.github.io/marathon/docs/rest-api.html)
    * API Reference for creating API calls and JSON application defintions 
* [MyHero Demo](https://github.com/hpreston/myhero_demo)
    * The templates and install scripts from MyHero can be good resources 

## IMPORTANT NOTE

As part of this bonus step, you will be posting your Tropo developer account info as part of your application definition.  It is **HIGHLY** recommended you reset your Tropo Password following the completion of this lab (to be safe).  And should your current Tropo Password be a common one you use on other sites, you may want to reset it **BEFORE** beginning this step.  

# Steps 

## Kill Locally Running App

The SMS service with Tropo is NOT designed to have multiple instances running under a single user account.  End the "dev" instance on your laptop.  

1. End all running containers with `Cntrl-C`

1. End running ngrok tunnel with `Cntrl-C`

## Publish Containers to Docker Hub 

In order to deploy the application to a cloud service, the Docker images need to be hosted in a reachable "artifact repository".  "Push" your images to Docker Hub.  

1. Login to hub.docker.com

    ```bash
    docker login 
    ```

1. Tag the web, api, and sms containers with your Docker Hub username.  

    ```bash
    docker tag haciendo_web:latest <YOUR DOCKER USER>/haciendo_web:latest
    docker tag haciendo_api:latest <YOUR DOCKER USER>/haciendo_api:latest
    docker tag haciendo_sms:latest <YOUR DOCKER USER>/haciendo_sms:latest
    ```    

1. Push the images to docker hub

    ```bash
    docker push <YOUR DOCKER USER>/haciendo_web:latest
    docker push <YOUR DOCKER USER>/haciendo_api:latest
    docker push <YOUR DOCKER USER>/haciendo_sms:latest
    ```

## Build Marathon Application Templates

You could certainly deploy your application to the DEVNET Sandbox using the GUI, but if you do everyone will laugh at you... Create JSON application definitions for each of three services. 

1. Create a new directory called `haciendo_aio/bonus-install` to hold installation templates.  

    ```bash
    mkdir bonus-install
    ```
    
2. Use the following example to create Marathon Application Definitions for the three services. 
    * *You will need to replace and customize the placeholders indicated by `{{ PLACEHOLDER }}` for each of the services*     

    ```json
    {
        "container": {
            "type": "DOCKER",
            "docker": {
                "image": "{{ DOCKER_USER }}/haciendo_web:latest",
                "forcePullImage": true,
                "network": "BRIDGE",
                "portMappings": [{
                    "containerPort": {{ EXPOSED PORT }},
                    "hostPort": 0
                }]
            },
            "forcePullImage": true
        },
        "healthChecks": [{
            "protocol": "TCP",
            "portIndex": 0
        }],
        "id": "/{{ DOCKER_USER }}/haciendo/{{ SERVICE }}",
        "instances": 1,
        "cpus": 0.1,
        "mem": 32,
        "env": {
            "{{ ENV VAR }}": "{{ VALUE }}"
        }
    }
    ```

## Deploy to DEVNET Mantl Sandbox

Use the REST API for Marathon to "POST" the JSON application definitions to the DEVNET Sandbox for each service.  It is suggested to create them in the order of sms -> api -> web, and to test each service independently as you create it. 

1. Deploy the sms service
    * It may take 1-3 minutes for the application to fully startup after running command
    * You can use a `GET` request to monitor for it to become healthy, or watch via the Web GUI

    ```bash
    curl -kX POST -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps \
        -H "Content-type: application/json" \
        -d @marathon-sms.json
    ```

1.  Test the containerized SMS Service
    * Insert your Docker Username into the command below
    * Insert your phone number (or if your number is NON-US a friends)

    ```bash
    curl http://<DOCKER_USER>-haciendo-sms.app.mantldevnetsandbox.com/hello/<YOUR PHONE NUMBER>  
    ```

1. Deploy the api service
    * It may take 1-3 minutes for the application to fully startup after running command
    * You can use a `GET` request to monitor for it to become healthy, or watch via the Web GUI

    ```bash
    curl -kX POST -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps \
        -H "Content-type: application/json" \
        -d @marathon-api.json
    ```

1.  Test the containerized API Service

    ```bash
    curl -X POST http://<DOCKER_USER>-haciendo-api.app.mantldevnetsandbox.com/api/score \
      -d 'line=hello' \
      -d 'phonenumber=55555555555'
    ```

1. Deploy the web service
    * It may take 1-3 minutes for the application to fully startup after running command
    * You can use a `GET` request to monitor for it to become healthy, or watch via the Web GUI

    ```bash
    curl -kX POST -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps \
        -H "Content-type: application/json" \
        -d @marathon-web.json
    ```

1. Access the new containerized version of the Web Interface by opening `http://<DOCKER_USER>-haciendo-web.app.mantldevnetsandbox.com` in a browser

1. Send a message to test that all is working

## Uninstall from the Mantl Sandbox

As cool as it is to have Haciendo running on the public Internet, you need to delete your running application when done.  Just like before, don't risk ridicule by using the GUI... 

1. Use the API to uninstall each service

    ```bash
    curl -kX DELETE -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps/<DOCKER_USER>/haciendo/web 
        
    curl -kX DELETE -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps/<DOCKER_USER>/haciendo/api 
        
    curl -kX DELETE -u admin:1vtG@lw@y \
        https://mantlsandbox.cisco.com:8080/v2/apps/<DOCKER_USER>/haciendo/sms         
    ```
   
## Go Reset your Tropo Password
 
As mentioned at the start of the lab, for the short time you worked on this bonus, your Tropo account was part of your application definition.  Go reset your password just in case... 
 
 