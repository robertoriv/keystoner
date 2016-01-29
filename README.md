[Keyston]e-Dock[er]
===================

## Introduction
The `Keystoner` repository contains a sample [KeystoneJS](http://keystonejs.com/) application, and supporting files for dockerizing it.

## Getting Started

### Requirements

The docker-compose files were tested against a Mac OS X (10.11.3) system, with the [Docker Toolbox](https://www.docker.com/products/docker-toolbox).

`KeystoneJS` requires: 
1. Yeoman (yo)
2. MongoDB

### Buiding the Images and Running the app

#### From Scratch (Build Your Own Image)

```
$ git clone git@github.com:robertoriv/keystoner.git keystoner
$ cd keystoner
$ docker-compose build
# grab some coffee?
$ docker-compose up
```

#### From DockerHub

```
$ git clone git@github.com:robertoriv/keystoner.git keystoner
$ cd keystoner
$ ddocker-compose -f docker-compose-from-dockerhub.yml up
```

### Testing that it Works

```
$ curl -I http://<docker_vm_ip|localhost>/assets/images/logo.svg 2>/dev/null | head -n 1 | cut -d$' ' -f2
200

$ curl -I http://<docker_vm_ip|localhost>/ 2>/dev/null | head -n 1 | cut -d$' ' -f2 
200

$ open http://<docker_vm_ip|localhost>/
```

## What's Included?

1. **nginx** - Used as a front-end to the app. nginx will proxy-pass requests to the app running in a container on port `3000`. In addition to proxy-passing the app request, it also hosts all the files/folders in the [nginx/assets](nginx/assets) directory. Those files will be served by nginx rather than the app. The docker image for nginx consists of the [offical nginx image](https://hub.docker.com/_/nginx/) with some added configuration. nginx is listening on port `80`.

2. **MongoDB** - [MongoDB](https://www.mongodb.org/) serves as the data store for the application. This image is an unmodified [official MongoDB image](https://hub.docker.com/_/mongo/). (More on that later.)

3. **Keystone Demo** - A basic KeystoneJS app.

## Issues (and Other Insteresting Findings)

### MongoDB 

Looking at the [docs for MongoDB's official image](https://hub.docker.com/_/mongo/), I noticed that it said that, when running on VirtualBox, you can't mount a local folder on the host machine.

> WARNING: because MongoDB uses memory mapped files it is not possible to use it through vboxsf to your host (vbox bug). VirtualBox shared folders are not supported by MongoDB (see docs.mongodb.org and related jira.mongodb.org bug). This means that it is not possible with the default setup using Docker Toolbox to run a MongoDB container with the data directory mapped to the host.

I decided to just ship an unmodified version of the image, which means that it will start with a blank slate. Not sure how you guys handle data migrations across environments, but this could prove to be a problem.

### Mounting the app as a Volume in the Container

When I mounted the app folder in the container to make it easier to see changes, it started complaining because my dev box had the dependencies installed for a different version of node. There was no image that matched my current version (installed with `brew`). I commented out the mount.

