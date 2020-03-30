# Docker

We have two types of docker paid (ee) or free (ce).



## Install docker on linux

**Insert more details when have time**

(red hat support only ee , centos will work with ce)

Three main ways to install :

1. run the command.

```bash
curl -sSL https://get.docer.com/ | sh
```

----







```bash
docker
```

will give you all the command of docker grouped by 

- management
- regular commands

```bash
#OUTPUT
#output

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  engine      Manage the docker engine
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```



```bash
#this command
docker container <container-command>
#can be replaced with this command
docker <container-command>
```



```bash
#verify cli can talk to engine
docker version

#most valuable config for engine
docker info
```



if you would like to get some help from docker about command

```bash
docker <command> # will give you some help with this command
docker <command> <sub-command> # will give you help with sub command

#equivilant to

docker <command> --help 
docker <command> <sub-command> --help 

```



```bash
docker network --help # get info for this command

# output
Usage:	docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
```



An image is the application we want to run

A container is an instance of that image running as process.

you can have many containers running of same image 

docker default registry is called Docker Hub.

docker registry is the place that docker pull the images.



```bash
docker run --publish 80:80 nginx
# equivlinat to
docker run -p 80:80 nginx
```

1. docker look for image ```nginx``` and pull the image from docker hub.
2. start a new container from that image.
3. opened port 80 on the host ip.
4. routes that traffic to the container ip , port 80



```bash
# will run the docker in detach mode ,  without -d if we exit terminal this kills the container.

docker run -d --publish 80:80 nginx
```



```bash
#list all running containers
docker ps
# equivliant to
docker container ls

# list all containers 
docker ps -a 
#or
docker container ls -a

# stop running container
docker stop <container-id>

# start new container
docker run 

# start existing container
docker start <container-id/name>
```

**docker container run always start new container**



example for output

```bash
# get running containers
aviad@aviad-Inspiron-7373:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS                      NAMES
b6a16b7bc340        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours                       0.0.0.0:27019->27017/tcp   mongo2
1f516193ecf9        mongo               "/usr/bin/mongod --b…"   8 months ago        Restarting (14) 39 seconds ago                              mongo1
96ed0d8a218e        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours                       0.0.0.0:27017->27017/tcp   mongo0

# stop running container
aviad@aviad-Inspiron-7373:~$ docker stop mongo1
# list running container , notice that mongo1 not exists
aviad@aviad-Inspiron-7373:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
b6a16b7bc340        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours          0.0.0.0:27019->27017/tcp   mongo2
96ed0d8a218e        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours          0.0.0.0:27017->27017/tcp   mongo0

# start again mongo1 
aviad@aviad-Inspiron-7373:~$ docker start mongo1 

# check that mongo1 is running
aviad@aviad-Inspiron-7373:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
b6a16b7bc340        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours          0.0.0.0:27019->27017/tcp   mongo2
1f516193ecf9        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 seconds        0.0.0.0:27018->27017/tcp   mongo1
96ed0d8a218e        mongo               "/usr/bin/mongod --b…"   8 months ago        Up 2 hours          0.0.0.0:27017->27017/tcp   mongo0
```



container have 2 things that unique :

- container id - id that generated when container created.
- container name - we can specify it , if we don't docker have large vocabulary and he will generate name.

you can notice that container named ```mongo2``` has container id of ```b6a16b7bc340```



### Create container

```bash
docker container run --publish 80:80 --detach --name webhost nginx
```



what happens when we run command

```bas
docker container run ?
```

1. looks for that image locally in image cache , doesn't find anything.
2. then looks remote image repository (defaults to docker hub)
3. downloads the latest version (if version is not specified)
4. Create new container based on that image and prepare to start.
5. Gives it a virtual IP on private network inside docker engine.
6. Opens up port 80 on host and forwords to port 80 in continaer.
7. start container by using CMD in the image Dockerfile.



remove container 

```bash
docker rm <container-id-1> <container-id-2> <container-id-3>
# or
docker delete <container-id-1> <container-id-2> <container-id-3>
```





```bash
docker run --name mongo -d mongo
```

```bash
docker top mongo
# list processes in containers
UID	PID		PPID	 c	 STIME TTY 	TIME	 		CMD
999	3883	3851	 1	 00:04  ?   00:00:00		mongod

# list processes on ou
ps aux | grep mongo

USER	PID 	%CPU 	%MEM 	VSZ		RSS 	TTY		START		TIME	COMMAND
aviad	3883	0		0		
```





so running container is a process while virtual machine is part of server that gets storage cpu and ram



homework

startup 3 containers :

- nginx
- mysql
- httpd (apache server)

run all of them with detach mode , and specify name.

nginx list to 80:80 , httpd on 8080:80 , mysql on 3306:3306.

pass to mysql environment variable ```MYSQL_RANDOM_ROOT_PASSWORD=yes```

the password will be in the logs 

eventully stop containers and then remove them.

ensure by docker ps -a that removed.



```bash
# CREATE CONTAINERS
docker run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql 
docker log mysql # to get password

docker run -d --name webserver -p 8080:80 httpd
docker ps # check that created and running

docker run -d --name proxy -p 80:80 nginx
docker ps # check that created and running

# DELETE CONTIANERS
docker stop <apache-container-id> <nginx-container-id> <mysql-container-id>
docker rm <apache-container-id> <nginx-container-id> <mysql-container-id>
```





What's going on in containers

```bash
# process list in one container
docker container top

# detials of one container config
docker container inspect

# performance stats for all container
docker container status 
```



```bash
docker stats
```

is just like htop to our containers.

```
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
b6a16b7bc340        mongo2              1.01%               173.3MiB / 15.41GiB   1.10%               18.8MB / 1.71GB     173MB / 62.9MB      66
1f516193ecf9        mongo1              0.00%               0B / 0B               0.00%               0B / 0B             0B / 0B             0
96ed0d8a218e        mongo0              0.96%               149.2MiB / 15.41GiB   0.95%               8.06MB / 53.7MB     52.9MB / 69.5MB     64
```





Getting inside docker 

```bash
docker run -it <container-name> <shell> # start new container interactivily
docker exec -it <container-name> <shell> # get inside running container
```

```bash
#get inside the container while creating and running it.
docker run -d -it -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql bash

# when we'll exit the shell the container will be stopped
```

```bash
#get inside the running container.
docker exec -it mysql bash
```

we can using shell that different from bash if the image contains it.

```bash
#here the shell will be ubuntu shell
docker run -it --name ubuntu ubuntu
```

```bash
# will get into existing but not running container when exists this will stop container
docker start -ai ubuntu 
```



### Docker Network

each container connected to private virtual network called brigde

each virtual network routes through NAT firewall on host IP

all containers on a virtual network can talk to each other without -p 

best practice is to create a new virtual network for each app

in docker you can easily change network for container at runtime.





if we don't specify network for container he we'll be in the bridge/docker0 network 

then will be able to talk to all rest container in this network.



you can't listen to more than one port in the host level.

```-p host-port:internal-port```



```bash
docker port <container-id>
# quick port check for container
```



Remember ```--publish``` or ```-p```  ?

We're going to dive in.



```bash
docker network ls # show networks
docker network inspect # detial about specific network
docker network create --driver # create new virtual network
docker netowrk connect # change a live running container
docker network dissconnect # change a live running container
```





```bash
aviad@aviad-Inspiron-7373:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
85955728d050        bridge              bridge              local
5998e36ac27e        code_default        bridge              local
09a0790fd0dd        docker_default      bridge              local
c675b5a43dc2        host                host                local
f70cccbe0f29        my_app_net          bridge              local
adbf81f6a2bb        mysql_default       bridge              local
8c7cbc3d7965        none                null                local

```



let's run new nginx container

```bash
docker run --publish 80:80 -d --name webhost nginx:alpine
```

now we check if this container connected to bridge

```bash
aviad@aviad-Inspiron-7373:~$ docker network inspect bridge 
[
    {
        "Name": "bridge",
        "Id": "85955728d05004f88bc9ef7e5e5e67a116bb3c23d0698af3682d5c10dd296508",
        "Created": "2020-03-30T08:21:31.443365206+03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "4f49df44ccb0c686cf006901e30ee9ff7a008132a2ea97a500b5ccc59ab6b923": {
                "Name": "webhost",
                "EndpointID": "cc2ce8314bc32fff1987bba0135585aeb45f2470e2f3216575c1e51ea3e5e16e",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]


# we can see all containers that connect to containers in field Containers

```



```bash
docker network create my_app_net1
# we'll check if the network is present

aviad@aviad-Inspiron-7373:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
85955728d050        bridge              bridge              local
5998e36ac27e        code_default        bridge              local
09a0790fd0dd        docker_default      bridge              local
c675b5a43dc2        host                host                local
f70cccbe0f29        my_app_net          bridge              local
21f0d3569ba2        my_app_net1         bridge              local
adbf81f6a2bb        mysql_default       bridge              local
8c7cbc3d7965        none                null                local

```



now we'll create new container and connect him to our new network

```bash
docker run --publish 81:80 -d --network my_app_net1 --name webhost_network nginx:alpine

# we can see that our container exists in the network
aviad@aviad-Inspiron-7373:~$ docker network inspect my_app_net1 
[
    {
        "Name": "my_app_net1",
        "Id": "21f0d3569ba2790375bdfe8c6f1adcc219dd0b91163b83920dc3bab59ee2d73f",
        "Created": "2020-03-30T11:59:52.805094915+03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.22.0.0/16",
                    "Gateway": "172.22.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "782a4c243b1282208230fa55e76f56cf1ca46c10a1f73993a25b6671f2fb1dd5": {
                "Name": "webhost_network",
                "EndpointID": "ef263aa5b39cd454a5fd3c07cbefdce9073d937ac23c1c5d223d7cda2cdfd43a",
                "MacAddress": "02:42:ac:16:00:02",
                "IPv4Address": "172.22.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

```



unplug exists container

```bash
docker network connect <network-id> <container-id>
docker network connect f70cccbe0f29 782a4c243b12

docker network disconnect <network-id> <container-id>
docker network connect f70cccbe0f29 782a4c243b12
```



in micro service world we cannot rely on ip address because ip address can change every minute.

when we create network we can talk to another container by his container name.



docker-compose create new network each time.





#### Assignment :

DNS round robin - each time you ask from the DNS for the server you gets different server.

1. create a new virtual network
2. create to containers from ```elasticsearch:2```  image.
3. research and use ```-network-alias search```

#### Solution :

```bash
docker network create aviados_network

docker run -d --name elasticsearch1 --net aviados_network -p 9200:9200 
--net-alias search elasticsearch:2

docker run -d --name elasticsearch2 --net aviados_network -p 9201:9200  
--net-alias search elasticsearch:2

docker run --rm --net aviados_network alpine:3.10 nslookup search
# OUTPUT
nslookup: can't resolve '(null)': Name does not resolve

Name:      search
Address 1: 172.23.0.2 elasticsearch1.aviados_network
Address 2: 172.23.0.3 elasticsearch2.aviados_network
```



# Docker Images

### what is it an image ?

We can understand what is image by notice what this is containing.

image contains :

- application binaries and dependencies.
- meta data of how to run the application.

**There is not OS No kernel modules because the host have those things.**



the official detention : 

"An image is an ordered collection of root file system changes and the corresponding execution parameters for use within a container runtime"



### DockerHub

DockerHub is a package images system for containers.

images without ```/``` is official images.

when we pull image notice the number of stars and pulls.

in docker hub we can go to explore and see all the official images.



### Image Layers

Images deranged like the onion file system design 

making layers about the changes.

```bash
#Get history of image layer
# if image does not have image id on this command this is not images by themself but layers on the image.
docker history nginx:latest 

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
540a289bab6c        5 months ago        /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  STOPSIGNAL SIGTERM           0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  EXPOSE 80                    0B                  
<missing>           5 months ago        /bin/sh -c ln -sf /dev/stdout /var/log/nginx…   22B                 
<missing>           5 months ago        /bin/sh -c set -x     && addgroup --system -…   57MB                
<missing>           5 months ago        /bin/sh -c #(nop)  ENV PKG_RELEASE=1~buster     0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  ENV NJS_VERSION=0.3.6        0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  ENV NGINX_VERSION=1.17.5     0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B                  
<missing>           5 months ago        /bin/sh -c #(nop)  CMD ["bash"]                 0B                  
<missing>           5 months ago        /bin/sh -c #(nop) ADD file:74b2987cacab5a6b0…   69.2MB              

```

Image is collection of layers :

Image 1

- Ubuntu
- Apt
- Env (environment variable change )

Image 2 

- Jessie
- Apt
- Copy
- Port

Image 3

- Jessie 
- Apt
- Env

Image 2 & 3 have common layers from bottom to up therefore they do not import and save new layer the will use the same layer to save time and space.



if container of image 3 change on running thing on Jessie image which common to container of Image 2 

what will happens is ```copy on write``` the file system will copy the image in the container layer now he will able to change without affect different containers.



When we pull image if layers exists as well docker won't pull this layers.



```bash
docker image inspect <image-name:tag>
# get meta data of image

docker image inspect nginx
[
    {
        "Id": "sha256:540a289bab6cb1bf880086a9b803cf0c4cefe38cbb5cdefa199b69614525199f",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:922c815aa4df050d4df476e92daed4231f466acc8ee90e0e774951b0fd7195a4"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2019-10-23T00:26:03.830480202Z",
        "Container": "77b8bfc5e16274066a5d4c14915ea5e7387c062f8540cd970c54e9b6e38b1011",
        "ContainerConfig": {
            "Hostname": "77b8bfc5e162",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.17.5",
                "NJS_VERSION=0.3.6",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:2e2fa75c52fdfe182fb66455d6db04849c683ef01d14a526211ba37831c66791",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGTERM"
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.17.5",
                "NJS_VERSION=0.3.6",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:2e2fa75c52fdfe182fb66455d6db04849c683ef01d14a526211ba37831c66791",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGTERM"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 126215561,
        "VirtualSize": 126215561,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/ad3a5e6c73ce9f273afa8419e9cc1bfcaaf444f43b56650c80c317c3678c8fa9/diff:/var/lib/docker/overlay2/5f4a4fac806c00e24c25dee1506053f28d60898701462d59943064edef2eb6fd/diff",
                "MergedDir": "/var/lib/docker/overlay2/b5444353f09548a13fd375b9116c3c8d710d6b77509b9fe0bf2d0f6eca0c0314/merged",
                "UpperDir": "/var/lib/docker/overlay2/b5444353f09548a13fd375b9116c3c8d710d6b77509b9fe0bf2d0f6eca0c0314/diff",
                "WorkDir": "/var/lib/docker/overlay2/b5444353f09548a13fd375b9116c3c8d710d6b77509b9fe0bf2d0f6eca0c0314/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:b67d19e65ef653823ed62a5835399c610a40e8205c16f839c5cc567954fcf594",
                "sha256:6eaad811af0237b78ba8b44a282d1564259d90007d628a032c5df7e3e2bbb613",
                "sha256:a89b8f05da3a2cbe459ef3fecfec8076fd0a7568db81f9164147b6f642e2dadf"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```



A container is just a single read/write layer on top of the image.



## Commands

```bash
# get installed images
docker image ls

docker pull <image:tag-version> # to download image

docker pull nginx # if we don't specify image version this will download the latest
docker pull nginx:1.11.9

```



same images will have the same image-id



### Tagging and Pushing

```bash
image ls
REPOSITORY                                      TAG                 IMAGE ID            CREATED             SIZE
nginxapp2                                       latest              35554602bc97        9 days ago          126MB
nginxapp1                                       latest              4f70b8c2a373        9 days ago          126MB
nginxapp                                        latest              1e07692424e3        9 days ago          126MB

```



The repository name is made from - ```user or organization name``` / ```repository```.

**only official** can be drop the user/organization name from repository name.

for example : ```aviadh/scrapy-flights```

the tag can be name.



```bash
docker image tag <image-name:tag> <image-name:tag>

docker image tag elasticsearch:2 aviad/nginx:testing
```



to push image we have to login.

```bash
aviad@aviad-Inspiron-7373:~$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: aviados
Password: *********
Login Succeeded

#or 
docker login -u "aviados" -p "*********" docker.io
```

to logout 

```bash
docker logout
```



when you login this save you details in ./docker/config 

this hash your user name and password if you don't want it to happen you just need logout after.



if you want it to be private you have to create the repository in docker hub first.

 

### Dockerfile



simple txt file with instructions of how to build the image.

docker engine will take the text and build an image from it.

is a automation of docker image creation.



how to do it ? 

create a file called Dockerfile

```touch Dockerfile```

```Dockerfile```

```dockerfile
# You have base image - to import base image use FROM command
# If you don't want base image import scratch
FROM alpine

# who maintain this image by MAINTAINER
MAINTAINER aviad hayumi <aviad.hayumi@gmail.com>

# run unix commands by RUN , executing only , running during building image
# usally we don't put a lot of run commands and what we will do is using operator & 
# because each run is a layer and we want our image be less layers as possible.
RUN apt-get update

# run command during docker creation there will be only one CMD command in Dockerfile
# will run on docker run / docker start
CMD ["echo","Helo World...! from first docker image"]

```



FROM - you have it to build an image , if you don't want to build your image upon image use scratch image which is empty image , this is the base layer for your imag

```bash
FROM <image-name>
FROM nginx
```



MAINTAINER - who responsible for this image

```bash
MAINTAINER text
MAINTAINER aviad <aviad@gmail.com>
```



RUN - commands that will be running in the process of creating the image ```docker build -t haha:11 .```

```bash
RUN command1 \
	&& command2 \
	&&command3 \
	&&command4

RUN npm build \
	&& npm test \
	&& npm install
```



CMD - will run when you will start the container ```docker run haha``` or ```docker start haha```

```bash
CMD [<executable>,<param1>,<param2>]

CMD ["echo","Helo World...! from first docker image"]
```



COPY - it copies files from host to docker image

```dockerfile
COPY <path from host> <path from docker new container>
```

what we doing here is to take the file a.txt and save it on ```/home/aviad/workspace```

```dockerfile
FROM ubuntu

RUN apt-get update

WORKDIR /home/aviad/workspace

COPY a.txt .
```



ADD



WORKDIR - imagine that you set all your files in path ```/home/aviad/workspace``` and each time you have to repeat this path.

this annoying , what you can do is specify your work directory to ```/home/aviad/workspace``` and then when commands will be running this will be on this directory.

```bash
WORKDIR <path>
WORKDIR /home/aviad/workspace
```



 

To create new image we have to have Dockerfile

```DockerFile```

```dockerfile
FROM debian:jessie 
# all images must have a FROM
# usually from a minimal Linux distribution like debian or (even better) alpine
# if you truly want to start with an empty container, use FROM scratch

ENV NGINX_VERSION 1.13.6-1~stretch
ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
# optional environment variable that's used in later lines and set as envvar when container is running

RUN apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
	&& \
	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
	found=''; \
	for server in \
		ha.pool.sks-keyservers.net \
		hkp://keyserver.ubuntu.com:80 \
		hkp://p80.pool.sks-keyservers.net:80 \
		pgp.mit.edu \
	; do \
		echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
	done; \
	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \
	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y \
						nginx=${NGINX_VERSION} \
						nginx-module-xslt=${NGINX_VERSION} \
						nginx-module-geoip=${NGINX_VERSION} \
						nginx-module-image-filter=${NGINX_VERSION} \
						nginx-module-njs=${NJS_VERSION} \
						gettext-base \
	&& rm -rf /var/lib/apt/lists/*
# optional commands to run at shell inside container at build time
# this one adds package repo for nginx from nginx.org and installs it

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log
# forward request and error logs to docker log collector


EXPOSE 80 443
# expose these ports on the docker virtual network
# you still need to use -p or -P to open/forward these ports on host

CMD ["nginx", "-g", "daemon off;"]
# required: run this command when container is launched
# only one CMD allowed, so if there are multiple, last one wins
```

```bash
docker image build -t <image-name:tag> <path-to-Dockerfile>
```



```bash
docker image build -t aviados/first-course-image .

#OUTPUT
Sending build context to Docker daemon  4.096kB
Step 1/7 : FROM debian:stretch-slim
 ---> c2f145c34384
Step 2/7 : ENV NGINX_VERSION 1.13.6-1~stretch
 ---> Using cache
 ---> 611143cd854f
Step 3/7 : ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
 ---> Using cache
 ---> 8ba9ff6ae548
Step 4/7 : RUN apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 	&& 	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; 	found=''; 	for server in 		ha.pool.sks-keyservers.net 		hkp://keyserver.ubuntu.com:80 		hkp://p80.pool.sks-keyservers.net:80 		pgp.mit.edu 	; do 		echo "Fetching GPG key $NGINX_GPGKEY from $server"; 		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; 	done; 	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; 	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* 	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list 	&& apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y 	nginx=${NGINX_VERSION} 						nginx-module-xslt=${NGINX_VERSION} 						nginx-module-geoip=${NGINX_VERSION} 						nginx-module-image-filter=${NGINX_VERSION} 						nginx-module-njs=${NJS_VERSION} 						gettext-base 	&& rm -rf /var/lib/apt/lists/*
 ---> Using cache
 ---> 654f41f00c28
Step 5/7 : RUN ln -sf /dev/stdout /var/log/nginx/access.log 	&& ln -sf /dev/stderr /var/log/nginx/error.log
 ---> Using cache
 ---> be5fd77d1177
Step 6/7 : EXPOSE 80 443
 ---> Running in 23227ce0b054
Removing intermediate container 23227ce0b054
 ---> 475fdcab7bf9
Step 7/7 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in b0fc2de88e48
Removing intermediate container b0fc2de88e48
 ---> c92ae4444bc2
Successfully built c92ae4444bc2
Successfully tagged aviados/first-course-image:latest

```

Notice that in some layers docker used chased this cause the very fast build.

When you crate a Dockerfile try to put all the changes in the end , to let docker use cache at most. 



```bash
#get info about host number images and size of them
docker system df


TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              55                  38                  14.87GB             5.219GB (35%)
Containers          119                 0                   567MB               567MB (100%)
Local Volumes       134                 69                  5.115GB             486.4MB (9%)
Build Cache         0                   0                   0B                  0B

```

```bash
docker system prune
# 
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] 

```



# Data Volumes

Containers are immutable re-deploy containers never changes.

OK nice to know but... what are we doing with database ?

Docker has two solution for this issue :

- Volumes - special location outside of container that shared to the container UFC.
- Mount - link container path to host path.



### Volumes

