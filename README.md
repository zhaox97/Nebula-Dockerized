# Overview
This repository serves as a potentially simpler way of installing and launching our Cosmos and Omniview web apps. This build of the project opts for using Docker as a means of virtualization. This way, the project can be launched without having to install all the necessary components to your local machine.

# Installation
For all platforms, all that is required for installation is a local copy of the project, and a Docker installation.
## OS X
 [Download for OS X](https://www.docker.com/docker-mac)
 
 After installation, run `docker version` and `docker run hello-world` in a terminal to verify that Docker is working.

## Windows
 [Download for Windows](https://www.docker.com/docker-windows)
 
 After installation, run `docker version` and `docker run hello-world` in a terminal to verify the Docker installation is working.

## Linux
 [Download for Linux](https://download.docker.com/linux/) 

 First, find the correct distro, verision, and processor type from the download link to get the correct package. Then, after setting the package path, run 

 `sudo dpkg -i /path/to/package.deb` 

to install. The Docker daemon starts automatically, so run `sudo docker run hello-world` in a terminal to verify the installation.

# User Guide

Once everything is installed, in a terminal, navigate to your `Cosmos-Dockerized/` folder (or any of its subdirectories) and run the command: 

`docker-compose up --build`

If you are just re-launching the project, using the `--build` tag is unnecessary. To stop running the project, run docker-compose down. 

# Developer Notes

When initialing cloning from git, be sure to either run `git clone` with the `--recursive` command, or run `git submodule init` followed by `git submodule update` to pull in all the submodules. When making any changes to the source code, the Docker containers must be restarted. Run `docker-compose down` and `docker-compose up --build` in order to apply the changes.

## Structure
This project is organized as a Docker project. The core pieces are as follows:

### Docker
Docker is the underlying virtualization software that runs the various apps in this project. In `Cosmos-Dockerized`, there is a `docker-compose.yml` file that serves as a configuration file for the Docker containers. It tells Docker which containers to build, port mapping instructions, and volume configuration, which is allows the virtual containers to read and write to local files. Each container also has its own `Dockerfile`, which serves as a set of build commands for the container. These files are good for establishing the virtual environment, as well as installing any external software necessary to run the project.

### neb/
This holds a `Nebula` repository, as well as the Dockerfile that sets up its container. Additionally, this folder holds a software requirements file, as well as a custom scikit-learn package needed for the project.

### omni/
This directory contains the Omniview repository and its Dockerfile.

### es/
This folder contains the Nebula-Elasticsearch repository and its Dockerfile.

### data/
This contains `bounds.json`, a file written to by `Omniview` and read by `Nebula` in order to define the coordinate bounds for the document search.


