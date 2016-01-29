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

```
$ git clone git@github.com:robertoriv/keystoner.git keystoner
$ docker-compose build
$ docker-compose up
```

## What's Included?

1. **nginx** - Used as a front-end to the app. nginx will proxy-pass requests to the app running in a container on port `3000`. In addition to proxy-passing the app request, it also hosts all the files/folders in the [nginx/assets](nginx/assets) directory. Those files will be served by nginx rather than the app. The docker image for nginx consists of the [offical nginx image](https://hub.docker.com/_/nginx/) with some added configuration.

2. **MongoDB** - [MongoDB](https://www.mongodb.org/) serves as the data store for the application. This image is an unmodified [official MongoDB image](https://hub.docker.com/_/mongo/). (More on that later.)

3. **Keystone Demo** - A basic KeystoneJS app.

## Issues (and Other Insteresting Findings)

### MongoDB 

Looking at the [docs for MongoDB's official image](https://hub.docker.com/_/mongo/), I noticed that it said that, when running on VirtualBox, you can't mount a local folder on the host machine.

> WARNING: because MongoDB uses memory mapped files it is not possible to use it through vboxsf to your host (vbox bug). VirtualBox shared folders are not supported by MongoDB (see docs.mongodb.org and related jira.mongodb.org bug). This means that it is not possible with the default setup using Docker Toolbox to run a MongoDB container with the data directory mapped to the host.

I decided to just ship an unmodified version of the image, which means that it will start with a blank slate.

### Mounting the app volume to the container

When I mounted the app folder in the container to make it easier to see changes, it started complaining because my dev box had the dependencies installed for a different version of node. There was no image that matched my current version (installed from `brew`). I disabled the mount.

