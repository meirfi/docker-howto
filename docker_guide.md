# Docker: Guide to Become a Docker Professional #

----------

Table of Content

- Docker Overview
- What is Docker
	- Docker vs VM
- Installing Docker
	- Choose Docker Type
		- CE - Community Edition
		- EE - Enterprise Edition
	- Choose Platform for installation
		- Centos
		- Ubuntu
		- Fedora
		- Debian
		- AWS
- Running Docker
- vUnderstanding Docker Run



## Docker Overview
## What is Docker
### Docker vs VMWare
## Installing Docker

## Running Docker
command : 

`
docker run [OPTION} IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]<br>

docker container run -d --name=nginx   -p 80:80 nginx:alpine<br>
docker container run -d --name=nginx-2 -p 81:80 nginx:alpine
`

### Docker Command Line

## vUnderstanding Docker Run
### topics
- Background and foreground containers
- Volumes with docker run

#### Docker run [Recap!]
`docker run [OPTION} IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`
#### Docker run with volume
- `docker container run --name website -v $(pwd):/usr/share/nginx/html -d -p 83:80 nginx:alpine`
- `docker container run --name website-forground -v $(pwd):/usr/share/nginx/html -p 84:80 nginx:alpine`
- `docker run --name ping --rm alpine ping -c 10 www.google.com`
- `docker run --name inside-container --rm -it alpine sh`

## Process Running Inside Docker Container and Resource Utilization
- docker top
- docker stat

### Docker top
- `docker top [CONTAINTER]`

- docker stat [OPTIONS] [CONTAINER...]

	`CONTAINER ID	NAME	CPU %	MEM USAGE / LIMIT	MEM %	NET I/O			BLOCK I/O	PIDS

	ccc0279dccb9	website	0.00%	1.406MiB / 15.67GiB	0.01%	115kB / 176kB	0B / 0B		2`

## Docker logs

### Command overview
Fetch the logs of a container

### Command Usage

Usage:  docker container logs [OPTIONS] CONTAINER

Command Options

-      --details        Show extra details provided to logs 
- -f,  --follow         Follow log output
-      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
-      --tail string    Number of lines to show from the end of the logs (default "all")
-  -t, --timestamps     Show timestamps<br>
       --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)


#### command example

- docker logs -f nginx<br>
- docker container logs -f nginx<br>
- docker container logs -f -t nginx<br>
- docker container logs -f -t --tail 10 nginx<br>
- docker container logs -f -t nginx<br>
- docker container logs -f -t --since 2017-06-14 nginx<br>
- docker container logs -f -t --since 2m nginx<br>
- docker container logs -f -t --since 2s nginx


## Docker Exec

### command overview
docker exec a command that run a new command on a running container

##### command lin
create new docker : docker run --name ubuntu --rm -it ubuntu bash
create exec file : docker exec -d ubuntu touch /tmp/execWorks 

#### login to docker
docker exec -it ubuntu bash


# Docker Volumes
### Docker Volumes Overview
Manage volumes

- Volumes are initialized when a container is created
- if the container's base image contains data at the specified mount point, that existing data is copied into the new volume upon volume initialization
- data volumes can be shared and reused among containers
- change to a data volume are made directly
- Changes to a data volume will not be included when you update an image
- data volumes persist even if the container itself is deleted


### Command Usage 

Usage:  docker volume COMMAND

Commands:

-  create      Create a volume
-  inspect     Display detailed information on one or more volumes
-  ls          List volumes
-  prune       Remove all unused local volumes
-  rm          Remove one or more volumes

Run 'docker volume COMMAND --help' for more information on a command.<br>



#### Docker volume creation


- `docker volume create -d local my-volume`

#### docker volume list

- `docker volume  ls`
output 
DRIVER              VOLUME NAME
local               my-volume

#### docker volume inspect

`docker-env:~/html# docker volume inspect  my-volume
[
    {
        "CreatedAt": "2018-06-14T11:16:47+03:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-volume/_data",
        "Name": "my-volume",
        "Options": {},
        "Scope": "local"
    }
]`

how to use this volume

`bash
docker-env:~/html# docker run -d -P -v my-volume:/webapp --name web training/webapp python app.py'
` 

show mount on docker

if jq command is not install just run `apt install jq`

`docker-env:~# docker inspect web | jq ".[].Mounts"`
`bash
[
  {
    "Type": "volume",
    "Name": "my-volume",
    "Source": "/var/lib/docker/volumes/my-volume/_data",
    "Destination": "/webapp",
    "Driver": "local",
    "Mode": "z",
    "RW": true,
    "Propagation": ""
  }
]
`

docker-env:~# docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
my-volume

Total reclaimed space: 0B




## Docker Update

- the docker update command dynamically  update container configuration
- you can use this command to prevent containers from consuming too many resources from their Docker host


### Command Usage

docker-env:~# docker update --help

Usage:  docker update [OPTIONS] CONTAINER [CONTAINER...]<br>

Update configuration of one or more containers<br>

Options:<br>
      --blkio-weight uint16        Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)<br>
      --cpu-period int             Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --restart string             Restart policy to apply when a container exits


`docker-env:~# docker run -d -p 8080:8080 --name routing albertogviana/docker-routing-mesh:1.0.0`
`docker-env:~# docker exec -it routing sh`
`docker-env:~# docker update --restart=always routing`

update docker memory: 

`docker-env:~# docker update --kernel-memory 80M routing`

## Docker System

### overview

Usage:  docker system COMMAND

Manage Docker

Commands:
  df          Show docker disk usage
  events      Get real time events from the server
  info        Display system-wide information
  prune       Remove unused data

Run 'docker system COMMAND --help' for more information on a command.


docker system
docker system df
docker system df -v
docker system events
docker system info
docker system prune
docker system df
docker system prune
docker system df
docker system prune --help
docker system prune -a
docker system df


## Docker network

### Overview
Manage networks


- It is the way to isolate the container network in a private network
- Isolates the database from private network
- Provides a DNS name


docker-env:~# docker network

Usage:  docker network COMMAND



Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.

- list available networks 

docker-env:~#  docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
7c1776c34457        bridge              bridge              local
b50a83b45963        host                host                local
730f750406ec        none                null                local

- create new network bridge 

`docker-env:~# docker network create -d bridge mybridge`
bde6b496825f82c7c80d311ae69c6bc215d530023da543c16216a9c4f5716cf0

- create host using our new bridge 

`docker container run -d --name nginx-bridge --network mybridge nginx:alpine`
docker run -it --rm --name alpine --network mybridge alpine wget -qO- nginx-bridge
docker run -d -p 80:80 --name nginx-bridge2 --network mybridge nginx:alpine
docker run -it --rm --name alpine --network none alpine ifconfig
docker run -it --rm --name alpine --network host alpine ifconfig


Docker Network Summary:

- Explored run container and check resource utilization
- Discussed process running in a container and learned about logs
- Explained how to execute commands in a container and volumes in Docker
- Updated container configurations
- Learned Docker management and network


# Docker Compose

### what is docker compose
- Compose is a tool for defining and running multi-container docker applications 
- With compose, you use a compose file to configure your application's services
- Then, using a single command, you create and start all the services from you configuration

### installing docker compose on linux system


1. Run this command to download the latest version of Docker Compose:
	- `curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose`

2. Apply executable permissions to the binary:
	- `sudo chmod +x /usr/local/bin/docker-compose`
3. Optionally, install command completion for the bash shell.
	- bash compilation
		- Place the completion script in /etc/bash_completion.d/
			- ```sudo curl -L https://raw.githubusercontent.com/docker/compose/1.21.2/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose```
4. Test the installation.
	- ```docker-compose --version```


### docker compose

> create new compose file via vim docker-compose.yml

`
docker-env:~# vi docker-compose.yml
version: '3'

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
`

* Running the docker via docker-compose command
`
docker-env:~# docker-compose up -d
Creating root_web_1 ... done
`

* see that the docker is up and running 
`
docker-env:~# docker-compose ps
   Name            Command          State         Ports
--------------------------------------------------------------
root_web_1   nginx -g daemon off;   Up      0.0.0.0:80->80/tcp
`