# EECE 5550 - Mobile Robotic

This repository contains material needed by homeworks and projects for Mobile Robotic class.

## Installation

### If you have docker and docker-compose already installed

```bash
# Cloning the repository
cd ~
git clone https://github.com/sharif1093/eece5550_master.git
cd eece5550_master
# To run the service (it will take a few minutes to run the docker the first time)
docker-compose run devel
# To build and run everything
docker-compose up --build
```

### Installing docker and docker-compose

* Follow instructions in [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/) to install docker on your machine. You should have `Ubuntu 16.04` or `Ubuntu 18.04`. If you have Ubuntu 14.04 follow instruction [here](https://gist.github.com/katopz/7eb4d8c475ee61e18624f3787c33fc21) instead (not tested).
* Add your user to the docker group:

```bash
sudo groupadd docker
sudo gpasswd -a $USER docker
```

* Install docker-compose by following instruction for linux [here](https://docs.docker.com/compose/install/#install-compose).


## Tips

* When inside the docker, you will have a working version of ROS Kinetic with osmesa driver installed.
* No changes in the files inside the docker will be preserved except changes done to the `/workspace` directory.
* If you need any extra packages to be installed, you should modify `deploy/Dockerfile` and add the package to the end of the file.


