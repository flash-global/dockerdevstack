Flash Europe International Development Stack
========

## Purpose
 As a development team, a lot  of tools are necessary for the development of the application.
 
 In order to facilitate the setup of the environment for the new developers and centralize the dependencies on each computer
 regardless of the OS, we created this repo.
 
 With this repository, we can now maintain easily our development stack.
 
## How it works
 
## Description
 Here are the description of the content of the project:
 * The `docker-compose.yml` is the main file. It contain all the description of the different services used commonly in all of our projects.
 * The `data` folder will contain all databases files for keeping persistence of their data even if the containers are shutdown.
 * The `env` file should contain all environment variable used in the `docker-compose.yml`file
 * The `traefik.toml` file contain the configuration of traefik.
 
## Prerequisites
 Docker and docker-compose needs to be installed on your machine. Check
 the [docker website](https://docs.docker.com/engine/installation/) for more informations.

## Usage
 1. Clone the project.
 2. Rename the `.env.example` to `.env`.
 3. Modify the .env file with the correct value if necessary.
 4. Run `docker-compose up` at the repository's root.
 5. If you use docker-compose for your other projects:
    1. Modify the projects' `docker-compose.yml` and add 
    ```
    dev_stack_net:
        external:
          name: dockerdevstack_dev-stack_net
    ```
    2. Add to an exposed webserver
    ```
    labels:
          traefik.frontend.rule: "Host:example.com"
          traefik.backend: "service-name"
          traefik.docker.network: "dockerdevstack_dev-stack_net"
    ```
    2. run `docker-compose up` in your project.
    3. Add the new host to your `/etc/hosts` (`127.0.0.1 example.com` in this case)
    4. You should now have access to the network of the dev stack in your project and the webserver should now be available at `example.com` 
 6. Profit!
 
## Todo
 * [ ] Update the `docker-compose` version from 2 to 3.
 * [ ] Add a more descriptive purpose section to the readme.

## Tools used
* [Mysql](https://www.mysql.com/) - Relational database
* [Docker](https://www.docker.com/) - Software container platform
* [Selenium](http://www.seleniumhq.org/) - Software-testing framework for web applications
* [Traefik](https://traefik.io/) - Modern HTTP reverse proxy and load balancer
* [InfluxDB](https://www.influxdata.com/) - Scalable datastore for metrics
* [Grafana](https://grafana.com/) - Feature rich metrics dashboard and graph editor

## Troubleshooting
On Linux, an issue is occurring when trying to start the docker.  
Traefik emits an error with port 80.  

Solution :  
- You need to check if another process is using the port 80.  
  `sudo netstat -plnt | grep 80`  
- If a process is using the port 80, you need to kill or just stop it. Before killing or stopping it, make sure that process isn't critical.  
  Stopping apache2 process example : `sudo systemctl stop apache2`   
- Now, you can start your containers by typing `docker-compose up` or `docker-compose up -d`  


On Mac, an issue is also occurring when trying to start the docker.
Traefik emits an error with port 80.

Solutions : 
- Check if an apache is already started on the machine :
    ``sudo apachectl stop``
    ``sudo /usr/sbin/apachectl stop``
- Rename your local folder docker-dev-stack to dockerdevstack
- Run this script to clean all of the docker : 
    ```
      # Stop all containers`
      docker stop `docker ps -qa`
      
      # Remove all containers
      docker rm `docker ps -qa`
      
      # Remove all images
      docker rmi -f `docker images -qa `
      
      # Remove all volumes
      docker volume rm $(docker volume ls -qf)
      
      # Remove all networks
      docker network rm `docker network ls -q`
      
      # Your installation should now be all fresh and clean.
      
      # The following commands should not output any items:
      # docker ps -a
      # docker images -a
      # docker volume ls
      
      # The following command show only show the default networks:
      # docker network ls
    ```
    
Note that these solutions don't all need to be tested to make it run.
