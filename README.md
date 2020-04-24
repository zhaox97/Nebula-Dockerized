# Overview
This repository serves as a potentially simpler way of installing and launching all our web applications. This build of the project opts for using Docker as a means of virtualization. This way, the project can be launched without having to install all the necessary components to your local machine.

# Installation
**NOTE:** The installation instructions below assume that all components can be run using Docker. However, Nebula-Elasticsearch currently fails to run properly within a Docker container. Using Nebula and Omniview with Docker and Nebula-Pipeline manually does not work. Because of this, manual installation and starting is required for each submodule. We hope to fix this in the future since Docker makes this process much easier.

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

Once Docker is installed, in a terminal, navigate to your `Cosmos-Dockerized/` folder (or any of its subdirectories) and run the command: 

`docker-compose up --build`

If you are just re-launching the project, using the `--build` tag is unnecessary. To stop running the project, run docker-compose down. 

# Developer Notes

When initialing cloning from git, be sure to either run `git clone` with the `--recursive` command, or run `git submodule update --init --recursive` to pull in all the submodules. You should verify that all submodules have been successfully downloaded (i.e., have files in them). The module/submodule structure is as follows:
```
Cosmos-Dockerized
    |
    Omniview
    |
    Nebula-Elasticsearch
    |
    Nebula
        |
        CosmosD3
        |
        Nebula-Pipeline
```

When making any changes to the source code, the Docker containers must be restarted. Run `docker-compose down` and `docker-compose up --build` in order to apply the changes. If a change is being made to the Dockerfile (or certain other changes), then `docker-compose build --no-cache` should be used to force Docker to completely rebuild each submodule/service. You can also add a submodule/service name (i.e., Nebula | Nebula-Pipeline | Omniview) to the end of the command to rebuild only one submodule/service, which may be useful since the --no-cache option means it will take longer to complete the build.

When developing code, keep in mind that each submodule is its own individual module/repository. As such, each submodule tracks its files separately from each other. This includes maintaining a separate list of branches and commits for each module. The way that the "parent" module tracks changes to its submodules is by tracking which commit it should point to for that submodule. For example, if you make a change to the Nebula-Pipeline sub-submodule (within the Nebula submodule), running `git status` in the Nebula submodule will indicate that changes have been made to the Nebula-Pipeline submodule with no further details. Similarly, running `git status` from the Cosmos-Dockerized repository will report changes to the `Nebula` submodule (since Nebula-Pipeline is nested within Nebula). To properly track this change, it will first need to be committed to the Nebula-Pipeline repository. Then, the Nebula repository should commit its Nebula-Pipeline submodule to properly track the new commit that was just made. Finally, Cosmos-Dockerized should commit its Nebula submodule to properly track the new commit for Nebula that tracks the Nebula-Pipeline commit/change. **Although this can seem tedious, it's important to ensure this tracking is done properly so that your collaborators know how you are altering your submodules. It also means they can run `git submodule update --recursive` to pull in all your changes at once.**

Because of the way that modules track their submodules, it means that you may encounted a **DETACHED HEAD state**. This state means that you are pointing to a specific commit separately from a branch. When you are in this state, you can neither push or pull code since git doesn't know which branch it should point to. To fix this issue, simply run `git checkout branchName` to tell git which branch you'd like to push/pull to.

## Structure
This project is organized as a Docker project. The core pieces are as follows:

### Docker
Docker is the underlying virtualization software that runs the various apps in this project. In `Cosmos-Dockerized`, there is a `docker-compose.yml` file that serves as a configuration file for the Docker containers. It tells Docker which containers to build, port mapping instructions, and volume configuration, which is allows the virtual containers to read and write to local files. Each container also has its own `Dockerfile`, which serves as a set of build commands for the container. These files are good for establishing the virtual environment, as well as installing any external software necessary to run the project.

### Nebula/
This holds a `Nebula` module/repository, which has the Dockerfile that sets up its container. Additionally, this repository holds a software requirements file, as well as a custom scikit-learn package needed for the project.

### Omniview/
This directory contains the Omniview module/repository and its Dockerfile.

### Nebula-Elasticsearch/
This folder contains the Nebula-Elasticsearch module/repository and its Dockerfile.

### data/
This contains `bounds.json`, a file written to by `Omniview` and read by `Nebula` in order to define the coordinate bounds for the document search.


