# Docker on OS X

## What is Docker?

> Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables apps to be quickly assembled from components and eliminates the friction between development, QA, and production environments. As a result, IT can ship faster and run the same app, unchanged, on laptops, data center VMs, and any cloud. ([Source](http://www.docker.com/whatisdocker))

## Difference between virtual machines and docker containers?

### Virtual Machine

> Each virtualized application includes not only the application - which may be only 10s of MB - and the necessary binaries and libraries, but also an entire guest operating system - which may weigh 10s of GB. ([Source](http://www.docker.com/whatisdocker))

### Docker Container

> The Docker Engine container comprises just the application and its dependencies. It runs as an isolated process in userspace on the host operating system, sharing the kernel with other containers. Thus, it enjoys the resource isolation and allocation benefits of VMs but is much more portable and efficient. ([Source](http://www.docker.com/whatisdocker))

## Installation on OS X

Docker utilizes Linux-specific kernel features, so OS X cannot be used for the Docker host. Fortunately, Docker allows you to use a Docker host that is different than your OS X system. Some instructions are borrowed from [here](https://docs.docker.com/installation/mac) and [here](http://blog.javabien.net/2014/03/03/setup-docker-on-osx-the-no-brainer-way).

### Homebrew (recommended)

It is recommended to use [Homebrew](http://brew.sh) as an OS X package manager. This will allow an easy way to install tool needed for Docker as well as common Linux utilities that do not come natively on OS X. It is assumed that Homebrew will be used for the remainder of this guide. Alternative links will be provided where appropriate.

```bash
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

### VirtualBox

VirtualBox is used to run the Docker host. It can be installed via the GUI installer found [here](https://www.virtualbox.org/wiki/Downloads). There is also a way to install VirtualBox via a tool called [Cask](http://caskroom.io). That will not be covered in this guide.

### Boot2Docker and Docker

Boot2Docker provides a convenient way to create a linux-based Docker host. This will be used to run our Docker containers since they cannot run natively on OS X. ([alternative installation](https://github.com/boot2docker/osx-installer/releases))

```bash
brew install boot2docker
```

A helper function has been provided called `docker-host` in the repository. This will aid in the managing of the Docker host. Boot2Docker already provides useful command line control, but this command will assist with setting a necessary environment variable needed for Docker to work. Source this in your `~/.bashrc` for convenience.

```bash
docker-host help
```

**_When the Docker host is destroyed, any downloaded Docker images are also destroyed. For example, if you did a_** `docker pull centos` **_and then destroyed the Docker host, you would have to pull the CentOS repository again._**

```bash
brew install docker
```

### Test Installation

We can test all the components by running a simple command. If your boot2docker VM does not already have the necessary image, it will be downloaded automatically. If you see 'Hello World!', everything is working properly.

#### Set DOCKER_HOST with helper function

```bash
docker-host # helper function (see above)
```

#### Set DOCKER_HOST with boot2docker

```bash
boot2docker init
boot2docker up
export DOCKER_HOST="tcp://$(boot2docker ip 2>/dev/null):2375"
```

#### Run Test

```bash
docker run centos echo 'Hello World!'
```

### Common Errors

#### /var/run/docker.sock: no such file or directory

This error means that DOCKER_HOST is not set correctly. It can be set via the helper function (see above).
