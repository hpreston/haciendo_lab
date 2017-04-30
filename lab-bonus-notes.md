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

## Publish Containers to Docker Hub 

In order to deploy the application to a cloud service, the Docker images need to be hosted in a reachable "artifact repository".  "Push" your images to Docker Hub.  

## Build Marathon Application Templates

You could certainly deploy your application to the DEVNET Sandbox using the GUI, but if you do everyone will laugh at you... Create JSON application definitions for each of three services.  

## Deploy to DEVNET Mantl Sandbox

Use the REST API for Marathon to "POST" the JSON application definitions to the DEVNET Sandbox for each service.  It is suggested to create them in the order of sms -> api -> web, and to test each service independently as you create it.  

## Uninstall from the Mantl Sandbox

As cool as it is to have Haciendo running on the public Internet, you need to delete your running application when done.  Just like before, don't risk ridicule by using the GUI... 
   
## Go Reset your Tropo Password
 
As mentioned at the start of the lab, for the short time you worked on this bonus, your Tropo account was part of your application definition.  Go reset your password just in case... 
 
 