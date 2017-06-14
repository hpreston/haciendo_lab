# Lab Step 1

![Lab 1][3]

[3]: refactor1.png "Move Web layer to Docker"

Refactor Web into a Docker container

* API Service continues to run in VM
* SMS Service continues to run in VM

# Steps 

## Vagrantfile Updates 

1. Destroy current Vagrant box 

    ```bash
    vagrant destroy
    ```

1. Don't map the Web Port
    * Delete/Comment ~line 26
    
        ```
        config.vm.network "forwarded_port", guest: 5080, host: 15080
        ```

1. Don't start the Web Service
    * Delete/Comment ~line 114
        
        ```bash
        # Start the Web Service
        cd /vagrant/web
        nohup python haciendo_web.py -p 5080 -a http://localhost:5000/api/score > web_log.log 2>&1 &        
        ```
        
1. Update comments/docs at end
    * Delete/Comment ~line 124
    
        ```bash
        echo "Web running on http://localhost:15080 "
        ```

1. Start new VM with just Web and SMS running
        
    ```bash
    vagrant up 
    ```            
        
1. Leave Vagrant VM up and running
         
## Docker File Creation 

1. Create new empty file called `Dockerfile` within the `./web` directory 

    ```bash
    cd web
    touch Dockerfile
    ```

1. Boilerplate python/3.5-alpine Dockerfile 

    ```
    FROM python:3.5-alpine
    
    # Install basic utilities
    RUN apk add -U \
            ca-certificates \
      && rm -rf /var/cache/apk/* \
      && pip install --no-cache-dir \
              setuptools \
              wheel    
    ```
    
1. Add web code and port details
  
    ```
    FROM python:3.5-alpine
    EXPOSE 80
    
    # Install basic utilities
    RUN apk add -U \
            ca-certificates \
      && rm -rf /var/cache/apk/* \
      && pip install --no-cache-dir \
              setuptools \
              wheel
    
    # Create application directories
    RUN mkdir /app \
      && mkdir /app/static \
      && mkdir /app/templates
    
    # Prep Required Environment Variables
    # These are default placeholders but should be overridden
    # when running the container
    ENV API_SERVER="http://localhost:5000"
    
    # Copy needed application requirements file into image
    ADD requirements.txt /app
    
    # Install Python requirements
    RUN pip install -r /app/requirements.txt
    
    # Copy the web templates and resources into the image
    COPY static /app/static
    COPY templates /app/templates
    
    # Copy application code into image
    ADD haciendo_web.py /app     
    ```
    
1. Add CMD to start Haciendo
        
        # Run the application
        CMD cd /app && python haciendo_web.py -p 80 -a ${API_SERVER}/api/score    
     ```        

## Build Docker image

1. Build the image

    ```
    cd web
    docker build -t haciendo_web . 
    ```

## Run Docker image

1. Determine your local workstation's IP address.  
    * Needed to provide the address for the API server to the Web Container
    * Can't use "localhost" because within the container, "localhost" means the container, not your workstation
    
    ```
    # Most Macs use en0 for Wifi, yours might be different
    ifconfig en0
    ```
    
    * Remember this IP for the next command
    
1. Run WEB container in interactive mode to view the startup and logs
    * Insert your workstation IP address into the command below

    ```
    docker run -it --env API_SERVER=http://<YOUR IP ADDRESS>:15000 -p 15080:80 haciendo_web 
    ```
    
1. Access the new containerized version of the Web Interface by opening `http://<YOUR IP ADDRESS>:15080` in a browser

1. Send a message to test that all is working

