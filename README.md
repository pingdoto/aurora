# Aurora

Aurora is an open source alternative to the OpenStack® Horizon dashboard, providing a modern customizable web user experience for IaaS and PaaS services.  
It’s currently being actively developed by [Cloudbase](http://www.cloudbase.it) and [Enter](http://www.entercloudsuite.com), looking for additional enterprise contributors and early adopter users.

## The Aurora Stack

Aurora UI is a brand new dashboard with a focus on customization and extensibility.  
Aurora API provides a layer of abstraction between the UI and an OpenStack infrastructure. The API Gateway is the entry point for the UI. It forwards client calls based on information provided by the Service Manager and following dynamically created routes.  
The core service is the main plugin that provides the "plumbings" to an OpenStack Infrastructure.  

<img src="infrastructure.png" width="80%">

## Components

| Name | Reporitory Link | CI |
| -- | -- | --|
| Web UI | [https://github.com/entercloudsuite/aurora-ui](https://github.com/entercloudsuite/aurora-ui) |[![Gateway Build Status](http://185.48.34.80/api/badges/entercloudsuite/aurora-ui/status.svg)](http://185.48.34.80/entercloudsuite/aurora-ui) |
| Gateway | [https://github.com/entercloudsuite/aurora-gateway](https://github.com/entercloudsuite/aurora-gateway) |[![Gateway Build Status](http://185.48.34.80/api/badges/entercloudsuite/aurora-gateway/status.svg)](http://185.48.34.80/entercloudsuite/aurora-gateway) |
| Manager  | [https://github.com/entercloudsuite/aurora-manager](https://github.com/entercloudsuite/aurora-manager) |[![Manager Build Status](http://185.48.34.80/api/badges/entercloudsuite/aurora-manager/status.svg)](http://185.48.34.80/entercloudsuite/aurora-manager) |
| Core | [https://github.com/entercloudsuite/aurora-core](https://github.com/entercloudsuite/aurora-core) |[![Core Build Status](http://185.48.34.80/api/badges/entercloudsuite/aurora-core/status.svg)](http://185.48.34.80/entercloudsuite/aurora-core) |

## Build your own UI

The aurora stack is built upon the microservices architecture. It is composed of three parts **backend** and one part **frontend**. 
If you don't like our UI, you can build your own with the language you want, with the tools you prefer and not care what's underneath the covers between you and Openstack: **it's all taken care of**.
Documentation for the integration aurora-ui and aurora-gateway is not available yet. If you wish to have more information about these feature, please don't hesitate to contact us or create an [issue](https://github.com/entercloudsuite/aurora/issues).

## Try Aurora (the easy way)


### What do you need
- **Docker® for Desktop**

### Running Aurora on Enter Cloud Suite

If you are looking an easy way to setup the Aurora stack, consider to try [Enter Cloud Suite](http://www.entercloudsuite.com) for an hosted cluster installation.
If you need help setting up the account visit the [Support Page](https://www.entercloudsuite.com/en/contact-us/) for more details.

### Infrastructure Quickstart 
**IMPORTANT:** for security and isolation reasons a new tenant or an empty region MUST be use in order to create the cluster

1. Clone the repo using your git client.  
2. Edit the [ **openrc.sh** ](/src/openrc.sh) with your account information (found inside the src folder)
3. `make start`: run the tool in a local Docker container, from where you can use the following commands:(docker has to build the image from the Dockerfile and it may take a while)  
    - `source openrc.sh` sets up the OpenStack client configurations (which was previously edited)
    - Verify that authentication is working properly by running an openstack command like: `openstack server list`.
    - `make create` starts the servers in your OpenStack project.  
    - `make deploy` starts the deployment of a Docker Swarm cluster on the running servers and provision all the Aurora stack.
    
Thanks to [Andrea Tosatto](https://github.com/atosatto) for his great [ansible role](https://galaxy.ansible.com/atosatto/docker-swarm/) which sets up the Docker stuff.  

## Listing the services

Use  `make login host=ansible-dockerswarm-manager` to log in to the manager node of the Docker Swarm cluster.
Inspect Docker Swarm to check all the services are running.  
The command `docker service ls` should show a set of services that look something like this:   

```
ID            NAME            MODE        REPLICAS  IMAGE
3sm9wb2csuzg  aurora_rabbit   replicated  1/1       rabbitmq:3-alpine
da4h3v6z93u4  aurora_manager  replicated  1/1       ecsdevops/aurora-manager:latest
ejcavvgrad25  aurora_ui       replicated  1/1       ecsdevops/aurora-ui:latest
gsesxl33mfkq  aurora_api      replicated  1/1       ecsdevops/aurora-gateway:latest
i0t34uhhumm6  aurora_redis    replicated  1/1       redis:alpine
lvie1hkgqckq  aurora_core     replicated  1/1       ecsdevops/aurora-core:latest
```

### Getting Started with Aurora

**It's time to load the Aurora Dashboard!**

Get the public IP address of one of your Docker Swarm nodes.
Open it with your browser, setting the port to **9000**. You should see the login page of Aurora, where you can sign in with your Enter Cloud Suite credentials.  
If the browser can't load the login page, be patient: the docker-engine needs to download the images for all the services before starting them. It will only take a few minutes. 

## How to cleanup after testing
1. if your exited the aurora container, just type `make start` as you did when creating the container the first time.
2. once inside the container, simply typing `make destroy` will delete the cluster and will stop paying for it

## Current UI Design
[https://marvelapp.com/1fai4ah/screen/16826137](https://marvelapp.com/1fai4ah/screen/16826137)

## Slack Channel
[https://aurora-devteam.slack.com](https://aurora-devteam.slack.com)

## Demo Video
[![asciicast](https://asciinema.org/a/c4g9mn8lf3bs5qjg2g2r8bfms.png)](https://asciinema.org/a/c4g9mn8lf3bs5qjg2g2r8bfms)
