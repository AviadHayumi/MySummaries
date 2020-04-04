# Docker

We have two types of docker paid (ee) or free (ce).

> insert details about docker and how works internally 





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



shortcut for docker container

```bash
#this command
docker container <container-command>
#can be replaced with this command
docker <container-command>
```



```bash
#verify cli can talk to engine
docker version

#most valuable config for docker engine
docker info
```



if you would like to get some help from docker about command

```bash
docker <command> # will give you some help with this command
docker <command> <sub-command> # will give you help with sub command

#equivalent to

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

# list all ruuning & not running containers 
docker ps -a 
#or
docker container ls -a

# stop running container
docker stop <container-id/name>

# start new container
docker run 

# start existing container
docker start <container-id/name>
```

**`docker container run` always start new container**



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

- container name - we can specify it , if we don't docker specify what will happen is that the container will gets default name.

  docker has  large vocabulary and he will generate name.

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
2. then looks remote image repository (defaults to docker hub).
3. downloads the latest version (if version is not specified).
4. Create new container based on that image and prepare to start.
5. Gives it a virtual IP on private network inside docker engine.
6. Opens up port 80 on host and forewords to port 80 in container.
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



so running container is a process on machine while virtual machine is part of server that gets storage cpu and ram.



### Assignment

startup 3 containers :

- nginx
- mysql
- httpd (apache server)

run all of them with detach mode , and specify name.

nginx listten to 80:80 , httpd on 8080:80 , mysql on 3306:3306.

pass to mysql environment variable ```MYSQL_RANDOM_ROOT_PASSWORD=yes```

the password will be in the logs 

eventually stop containers and then remove them.

ensure by docker ps -a that removed.



### Assignment Result

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

each container connected to private virtual network called bridge

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
docker network disconnect f70cccbe0f29 782a4c243b12
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

ok so if we get in `mysql` Dockerfile we can notice that we have an layer the contains command

```dockerfile
VOLUME /var/lib/mysql
```



so what is it means ?

let's install `mysql`

```bash
docker run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
```

then let's inspect

```bash
docker container inspect mysql    
```

```json
//we can notice two fields 
  "Mounts": [
            {
                "Type": "volume",
                "Name": "33d7a1afb3fa93b551a8309d6659f77a2f3290d5a4a2e416639be9fd007f34a3",
                "Source": "/var/lib/docker/volumes/33d7a1afb3fa93b551a8309d6659f77a2f3290d5a4a2e416639be9fd007f34a3/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
  ]
        
"Volumes": {
    "/var/lib/mysql": {}
}
```

the container is giving special space on the host to save his data 

in this case ```Mounts.0.Source``` represent the place on the host that the data is being saved.

```Mounts.0.Destination``` is the place of the data in the container.

if we are doing 

```bash
docker volume inspect 33d7a1afb3fa93b551a8309d6659f77a2f3290d5a4a2e416639be9fd007f34a3
```

we can get data on the specific volume

```json
[
    {
        "CreatedAt": "2020-03-30T21:54:51+03:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/33d7a1afb3fa93b551a8309d6659f77a2f3290d5a4a2e416639be9fd007f34a3/_data",
        "Name": "33d7a1afb3fa93b551a8309d6659f77a2f3290d5a4a2e416639be9fd007f34a3",
        "Options": null,
        "Scope": "local"
    }
]
```

in Linux machine you can navigate to the host path.

if we delete container the volume still exists.

volume need manual deletion 

```bas
docker run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
```

notice ``` -v <-volume-firendly-name->:<path-in-host-machine>```

this will create it in ```/var/lib/docker/volumes/mysql-db/_data```

the defaults is in ```/var/lib/docker/volumes/volume-name/_data```

to make it more friendly we can name volume.



## BInd Mounting

you cant specify mounting in docker file but you only can specify them while creating docker from cli.

Maps a host file or directory to a container file or directory 

Basically just two locations pointing to the same file

this skip UFS and host files overwrite any in container.



what we will do is to overwrite nginx html page.

```bash
docker run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
```

in /use/share/nginx/html nginx hold the html file to present what we did is to change this dir to our host dir that contains diffrent html file.

now diffrent html page will be present.

```html
curl localhost:80

<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>HEll YEAAA!</title>
</head>
<body>
  <h1>This is me overwrite nginx html :) !</h1>
</body>
</html>
```



### Volume Attachment

we are going to update database and let the data being exists.

1. Create postgres container with voulme psql-data using version 9.6.1.
2. use docker hub to learn VOLUME path versions needed to run it.
3. check logs , stop container, check if docker vulme ls contains your volume.
4. Create a new postgres container with same named voulme using 9.6.2

```bash
docker run -d --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=aviad -v my-lovely-posgress-vol:var/lib/postgresql/data postgres:9.6.1

docker volume ls 

DRIVER              VOLUME NAME
local               my-lovely-posgress-vol

docker run -d --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=aviad -v my-lovely-posgress-vol:/var/lib/postgresql/data postgres:9.6.2

docker stop my-postgres

#by doing this check
docker inspect my-postgres
docker inspect my-postgres1

#check in mount field there are reffer to same volume
```





```
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_PASSWORD=mypasswd
```



# Docker Compose

When we think about container it just a single process solution.

what happens if need couple of containers to get to our solution ?

Docker Compose allow us 

- configure relationships between containers.
- make our docker settings file easy to read.
- create one place for environment setup for the developer.



we have 2 parts of docker-compose :

- yaml file , how to setup our containers.
- cli named ```docker-compose``` to manage this file.

```docker-compose.yml```

```yaml
version: '3.1'  # if no version is specificed then v1 is assumed. Recommend v2 minimum

services:  # containers. same as docker run
  servicename: # a friendly name. this is also DNS name inside network
    image: # Optional if you use build:
    command: # Optional, replace the default CMD specified by the image
    environment: # Optional, same as -e in docker run
    volumes: # Optional, same as -v in docker run
  servicename2:

volumes: # Optional, same as docker volume create

networks: # Optional, same as docker network create
```



example 1 :

```yaml
version: '2'

# same as 
# docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve

services:
  jekyll:
    image: bretfisher/jekyll-serve
    volumes:
      - .:/site
    ports:
      - '80:4000'
```



example 2 :

```yaml
version: '2'

services:

  wordpress:
    image: wordpress
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: example
      WORDPRESS_DB_PASSWORD: examplePW
    volumes:
      - ./wordpress-data:/var/www/html

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: examplerootPW
      MYSQL_DATABASE: wordpress
      MYSQL_USER: example
      MYSQL_PASSWORD: examplePW
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

**version 2 notice that ``environment`` is a key value , ```ports & volumes``` are list**



example 3 :

```yaml
version: '3'

services:
  ghost:
    image: ghost
    ports:
      - "80:2368"
    environment:
      - URL=http://localhost
      - NODE_ENV=production
      - MYSQL_HOST=mysql-primary
      - MYSQL_PASSWORD=mypass
      - MYSQL_DATABASE=ghost
    volumes:
      - ./config.js:/var/lib/ghost/config.js
    depends_on:
      - mysql-primary
      - mysql-secondary
  proxysql:
    image: percona/proxysql
    environment: 
      - CLUSTER_NAME=mycluster
      - CLUSTER_JOIN=mysql-primary,mysql-secondary
      - MYSQL_ROOT_PASSWORD=mypass
   
      - MYSQL_PROXY_USER=proxyuser
      - MYSQL_PROXY_PASSWORD=s3cret
  mysql-primary:
    image: percona/percona-xtradb-cluster:5.7
    environment: 
      - CLUSTER_NAME=mycluster
      - MYSQL_ROOT_PASSWORD=mypass
      - MYSQL_DATABASE=ghost
      - MYSQL_PROXY_USER=proxyuser
      - MYSQL_PROXY_PASSWORD=s3cret
  mysql-secondary:
    image: percona/percona-xtradb-cluster:5.7
    environment: 
      - CLUSTER_NAME=mycluster
      - MYSQL_ROOT_PASSWORD=mypass
   
      - CLUSTER_JOIN=mysql-primary
      - MYSQL_PROXY_USER=proxyuser
      - MYSQL_PROXY_PASSWORD=s3cret
    depends_on:
      - mysql-primary
```

**version 3 ```enivorment&ports&volumes``` are lists. ** 

depends_on specify that we are depending on another service.



if we don't specify volumes or networks

## Docker Compose cli

```bash
docker-compose up #setup all volumes,networks,start all containers
docker-compose down # remove volumes,networks and stop all containers
docker-compose -d up #start with detach mode
docker-compose logs # see logs of containers
docker-compose top # list all containers processes
docker-compose --help # get all info about commands
```

in the background docker-compose talking to docker API.



### Assignment 

create docker-compose.yml from scratch 

- use drupal image expose on port 8080 so you can call localhost:8080
- use POSTGRES_PASSWORD for postgres
- docker-compose up 
- give the drupal postgres service name

```yaml
version: '3'

services:
  drupal:
    image: drupal
    ports:
      - "8080:80"
  postgres:
    image: postgres
    ports:
     - "5432:5432"
    environment: 
      - POSTGRES_PASSWORD=pass
```



## Build image in compose file

```yaml
version : '2'

services:
	proxy:
		build:
			context: . #path of image Dockerfile
			dockerfile: nginx.Dockerfile #name of Dockerfile file
		image: nginx-custom #how do we call the image
		ports:
		  - '80:80'
	web:
		image: httpd
		volumes: 
		  - ./html:/user/local/apache2/htdocs
```

when we will execute this 

first docker-compose check for the image `nginx-custom` in the cache , if not find it then create it.



docker compose name the networks , volumes , build images (if name not specify) with the name of directory project name.



```Dockerfile```

```dockerfile
FROM drupal:8.8.2
RUN apt-get update && apt-get install -y git
RUN rm -rf /var/lib/apt/lists/*
WORKDIR /var/www/html/themes
RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git
RUN chown -R www-data:www-data bootstrap
WORKDIR /var/www/html
```

```docker-compose.yml```

```yaml
version: '2'

services:
  drupal:
    build:
      context: .
      dockerfile : Dockerfile
    image: custom-drupal
    ports:
      - "8080:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles       
      - drupal-sites:/var/www/html/sites      
      - drupal-themes:/var/www/html/themes
  postgres:
    image: postgres:12.1
    environment:
      - POSTGRES_PASSWORD=mypasswd

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
```



# Docker Swarm

When we want to scale and we put a lot of containers in same/different servers how would us maintain this?

- How do we automate container life cycle ?
- How can we easily scale out/in/up/down ? 
- How we ensure containers are re-created if they fail ?
- How can we replace containers without downtime ?
- How do we track out containers ?
- How can we create cross-node virtual networks ?
- How can we ensure only trusted servers runs our container ?
- How can we store secrets,keys,passwords and get them to the container ?



Swarm is a cluster of machines that runs docker ,scalable ,reliable platform running many containers.



Swarm is a clustering solution of write one file management and let it spread to number of servers.

We can orchestrate the life cycle of the containers.

Since 2017 swarm is built in Docker Eco-system.

to use swarm we need to enable it.



how to check if swarm is enable ?

```bash
docker info

# check that 
# Swarm: active   ==== V
# Swarm: inactive ==== X
```

to activate in case of incative

```bash
docker swarm init
```



see all nodes

```bash
aviad@aviad-Inspiron-7373:~$ docker node ls
ID                            HOSTNAME              STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
lu1zvijkco9yqf88w2noxv6u7 *   aviad-Inspiron-7373   Ready               Active              Leader              19.03.6
```



let's create a service that ping to google

```bash
docker service create alpine ping 8.8.8.8
```

now let's check if was created

```bash
aviad@aviad-Inspiron-7373:~$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
mmq81s8hkwvz        jolly_keldysh       replicated          1/1                 alpine:latest       

#we also can see that container have been created

aviad@aviad-Inspiron-7373:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
fe87bad85b5a        alpine:latest       "ping 8.8.8.8"      39 seconds ago      Up 38 seconds                           jolly_keldysh.1.f4wsy4awikm3n3fn3z9q88ctr
```

notice that the service generate name and put the name in container with some extra names.

```bash
aviad@aviad-Inspiron-7373:~$ docker service  update mmq81s8hkwvz --replicas 3
mmq81s8hkwvz
overall progress: 3 out of 3 tasks 
1/3: running   [==================================================>] 
2/3: running   [==================================================>] 
3/3: running   [==================================================>] 
verify: Service converged 


# we can see that we have in replica 3/3 , now if we'll put one of the containers down it automaticlly put new container up.

aviad@aviad-Inspiron-7373:~$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
mmq81s8hkwvz        jolly_keldysh       replicated          3/3                 alpine:latest       

```



docker service replace docker run command is swarm world, it allows us extra features likes replica 



## Create Swarm Cluster

get in to this website

 https://labs.play-with-docker.com/p/bq1u97joudsg008cnli0#bq1u97jo_bq1uai5im9m0009ed670

this if free don't worry, and create 3 instances



so we need to run this command

```bash
docker swarm init --advertise-addr ip-adress

$ docker swarm init --advertise-addr 192.168.0.48

as a respone we got this
# docker swarm join --token SWMTKN-1-0532yrzs20ryj51i8ibacn13ijv7vzfgk01at5eldrs8zon1ag-epo0dwpzir7i44l8rsuguao27 192.168.0.48:2377 
```

now we'll go to our another servers and run the response command that we've got.



only managers run swarm commands , you can promote worker to manager so don't worry .

```bash
docker node update --role manager <hostname>
```

if we want to add node as default as manager

```bash
docker swarm join manager--token SWMTKN-1-0532yrzs20ryj51i8ibacn13ijv7vzfgk01at5eldrs8zon1ag-epo0dwpzir7i44l8rsuguao27 192.168.0.48:2377 
```



### Swarm networking

To create swarm wide bridge network where containers host on the same virtual network can like accesses each other like they are in a vlan.

```bash
docker network create --driver overlay your-network-name
```



let's use what we were doing in drupal and postgres in docker swarm.

```bash
# let's create a network
docker network create --driver overlay mydrupal

# let's create service named psql of postgres
[node1] (local) root@192.168.0.13 ~
$ docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres
4vwbqh1n85sh319wufrzqgkvp
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 

[node1] (local) root@192.168.0.13 ~
$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
4vwbqh1n85sh        psql                replicated          1/1                 postgres:latest     

# let's see where the container was created
[node1] (local) root@192.168.0.13 ~
$ docker service  ps psql 
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
fspitp9lvdcu        psql.1              postgres:latest     node1               Running             Running 3 minutes ago                       

#ahaha! in node1

# lets create drupal
[node1] (local) root@192.168.0.13 ~
$ docker service create --name drupal --network mydrupal -p 80:80 drupal
0pfxf3sysn1tlbx0dz5qky8ek
overall progress: 1 out of 1 tasks 
1/1: running   

# let's check where drupal was created
[node1] (local) root@192.168.0.13 ~
$ docker service ps drupal 
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
[node1] (local) root@192.168.0.13 ~
$ docker service ps drupal 
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
65vslw6j13sb        drupal.1            drupal:latest       node3               Running             Running 50 seconds ago                       
# ahaha! in node3

```

now we can notice that if you'll try to get 

node1:80 , node2:80 , node3:80 , we'll get same site although the container is running on node3.

swarm doing the load balancing automatically.

you don't have to worry where is container exists.



###	Attatchment

# Assignment: Create A Multi-Service Multi-Node Web App

## Goal: create networks, volumes, and services for a web-based "cats vs. dogs" voting app.
Here is a basic diagram of how the 5 services will work:

![diagram](./architecture.png)
- All images are on Docker Hub, so you should use editor to craft your commands locally, then paste them into swarm shell (at least that's how I'd do it)
- a `backend` and `frontend` overlay network are needed. Nothing different about them other then that backend will help protect database from the voting web app. (similar to how a VLAN setup might be in traditional architecture)
- The database server should use a named volume for preserving data. Use the new `--mount` format to do this: `--mount type=volume,source=db-data,target=/var/lib/postgresql/data`

### Services (names below should be service names)
- vote
    - **bretfisher/examplevotingapp_vote**
    - web front end for users to vote dog/cat
    - ideally published on **TCP 80**. **Container listens on 80**
    - **on frontend network**
    - **2+ replicas** of this container

- redis
    - **redis:3.2**
    - key/value storage for incoming votes
    - no public ports
    - **on frontend network**
    - **1 replica** 

- worker
    - bretfisher/examplevotingapp_worker:java
    - backend processor of redis and storing results in postgres
    - no public ports
    - **on frontend and backend networks**
    - **1 replica**

- db
    - **postgres:9.4**
    - one named volume needed, pointing to **/var/lib/postgresql/data**
    - **on backend network**
    - **1 replica**
    - remember set env for password-less connections **-e POSTGRES_HOST_AUTH_METHOD=trust**

- result
    - bretfisher/examplevotingapp_result
    - web app that shows results
    - runs on high port since just for admins (lets imagine)
    - so run on a high port of your choosing (I choose **PORT 5001**), container listens on 80
    - **on backend network**
    - **1 replica**



## Result

```bash
# create networks
docker network create --driver overlay backend
docker network create --driver overlay frontend

docker service create --name vote-app --network frontend  --replicas 3-p  80:80 bretfisher/examplevotingapp_vote

docker service create --name reddis --network frontend  --replicas 1 redis:3.2

# this has to be connected to 2 networks
docker service create --name woker --network frontend --network backend --replicas 1  bretfisher/examplevotingapp_worker:java

docker service create --name db --network backend -e POSTGRES_HOST_AUTH_METHOD=trust 
--replicas 1 --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4

docker service create --name result --network backend -p 5001:80 --replicas 1 bretfisher/examplevotingapp_result

```



## Swarm Stacks

Stacks is the docker compose of docker services.

instead of writing command after command of create/running service , everything exists in one file , when we deploy it this run all services.



you don't have built in docker swarm file , only deploy.

when you run this file in your local machine this will ignore the deploy part.

when on swarm this will ignore build.



```voiting.yml```

```yml
version: "3"  # this have to be up to version 3
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 1
      update_config: # when i update what do i want to happen
        parallelism: 2 # only two conccurent can be updated
        delay: 10s
      restart_policy:
        condition: on-failure # restart when fail 
  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
    deploy:
      placement:
        constraints: [node.role == manager] # condition of where to deploy
  vote:
    image: bretfisher/examplevotingapp_vote
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: bretfisher/examplevotingapp_result
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: bretfisher/examplevotingapp_worker:java
    networks:
      - frontend
      - backend
    depends_on:
      - db
      - redis
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:
```



to lunch it 

```bash
docker stack deploy -c voiting.yml voteapp
```



```bash
[node1] (local) root@192.168.0.7 ~
$ docker stack deploy -c example-voiting-app-stack.yml voteapp
Creating network voteapp_default
Creating network voteapp_frontend
Creating network voteapp_backend
Creating service voteapp_visualizer
Creating service voteapp_redis
Creating service voteapp_db
Creating service voteapp_vote
Creating service voteapp_result

```

see all stacks

```bash
$ docker stack ls
NAME                SERVICES            ORCHESTRATOR
voteapp             6                   Swarm
```

see replica and all services

```bash
[node1] (local) root@192.168.0.7 ~
$ docker stack ps voteapp 
ID                  NAME                   IMAGE                                       NODE                DESIRED STATE       CURRENT STATE            ERROR                  
     PORTS
g99slb8v7at1        voteapp_worker.1       bretfisher/examplevotingapp_worker:java     node1               Running             Running 8 minutes ago                           
     
l6ol7hojrjwf         \_ voteapp_worker.1   bretfisher/examplevotingapp_worker:java     node1               Shutdown            Failed 8 minutes ago     "task: non-zero exit (1
)"   
yvpg8e62svlb        voteapp_result.1       bretfisher/examplevotingapp_result:latest   node2               Running             Running 36 minutes ago                          
     
gjtf2weaudb1        voteapp_vote.1         bretfisher/examplevotingapp_vote:latest     node2               Running             Running 36 minutes ago                          
     
u3hyishevl82        voteapp_db.1           postgres:9.4                                node1               Running             Running 36 minutes ago                          
     
u4fldt9iid9u        voteapp_redis.1        redis:alpine                                node3               Running             Running 36 minutes ago                          
     
3q7twpuk1dw6        voteapp_visualizer.1   dockersamples/visualizer:latest             node1               Running             Running 35 minutes ago                          
```



we have in port 8080 all the nodes with services.



when we want to update we just need to run the deploy command , this will deploy only the changes.



### Swarm Secrets

secrets is all the data that you want to hide and protect like username,password,twitter api key ,ssh key , etc..



Swarm raft db is encrypted on your disk as default (when you do docker swarm init).

manager talks to worker through tls layer 

only containers assign to swarm can see them.

Secrets depend on swarm , we can either use secrets on our machine with docker compose but this is not secure.



we can create secrets in 2 ways : 

- create a file and give it to swarm.
- pass key value through cli.



```bash
$ echo aviad >> psql_user.txt

[node1] (local) root@192.168.0.7 ~
$ docker secret create psql_user psql_user.txt 

#output
z3irhefm7drmxf74gucf56vwh
```



```bash
[node1] (local) root@192.168.0.7 ~
$ echo "myDBPassword" | docker secret create psql_pass -
wq6ysl4umjybk78hatfyt76w6
```



two ways insecure 

in the first user just can access the file and see the content

the second option , if somone does histroy he can see this passord.



see existing secrects 

```bash
[node1] (local) root@192.168.0.7 ~
$ docker secret ls
ID                          NAME                DRIVER              CREATED             UPDATED
wq6ysl4umjybk78hatfyt76w6   psql_pass                               3 minutes ago       3 minutes ago
z3irhefm7drmxf74gucf56vwh   psql_user                               4 minutes ago       4 minutes ago
```



this is stored now in raft db , we can use inspect to see info like created , modifie 



secrets is being saved on /run/secrets/<encrypted-hash-secert>



so when we'll want to access the value from docker-compose

we should check if our image support take value from file

```    yaml
 POSTGRES_PASSWORD_FILE=/run/secrets/psql_password
```

 if image not support we need to take value from there in another way.



secrets and stacks

version has to be up to 3.1

```yaml
secrets:
  psql_user:
  	file: ./myuserfile.txt
  psql_password:
  	file: ./mypasswordfile.txt
```

we can either create the secret before and pass his encrypted value.



## Swarm Updates

if you want to update swarm in run time



update image

```bash
docker service update --image myapp:1.2.1 service_name
```

add env

```sbash
docker service update 	--env-add KEY=value service_name
```

remove port

```sbash
docker service update 	--publish-rm port service_name
```

change scale	

```sbash
docker service scale servicename=numbers
```



## HelthChecks

 Supported from :

- Dockerfile
- Compose YAML
- Docker run
- Swarm Services

this is simple command that has to bee running and return exit code of 0 or 1

0 - good thing

1 - error 



we have three states for container :

- starting
- healthy
- unhelathy

thins will be running every 30 seconds



docker container ls - we'll be able to check health of container.

docker container inspect - will hold 5 last health check result.



docker run will not handle unhealth container but docker swarm will

service will be replace task in case of fail health check.

```bash
docker run \
	--health-cmd = "curl -f localhost:9200/_cluster/health || false" \
	--health-interval = 5s \
	--health-retreis  = 3 \
	--health-timout   = 2s \
	--health-start-period = 15s \ 
  elasticsearch:2
```



interval - how often check will be made (default 30sec)

timeout - how long is going to wait before it errors out (default 30sec)

start-period - start time that check not relevant (default 0 ) usually for long build.

retries - try x time before deciding unhealthy

cmd - command the will be run to check (default ```curl -f http://localhost/ || false```)  **?**

​	

```yaml
version : "2.1" # minimum for health check
services : 
  web : 
    image : nginx
    healthcheck:
      test: ["CMD","curl","-f","http://localhost"]
      interval : 1m30s
      timeout : 10s
      retries : 3
      start_period : 1m # only supproted in 3.4 version
```



# Docker Registry

we can in DockerHub make image private.

you can use web-hook to let automation tool builds.

in collaborations we can put permissions for users to accesses our image.

you also have organization.

if you are using github or bitbucket you can create automatic build from docker hub, this will create ci based on your code commits.

each time you commit this is built in github.

you can base your image on another image (from) when the base image will be change this will change your image.





## Private Registry

we are going to create private registry 

notice that docker talk only with http**s** so what we are going to do is put registry on our local machine without tls layer this is only works on localhost.

```bash
docker run -d -p 5000:5000 --name registry registry

docker pull hello-world

docker tag hello-world 127.0.0.1:5000/hello-world
docker push 127.0.0.1:5000/hello-world # will push to local registry 
```



this is not common not use volume in this case , there are serval options :

- using volume
- s3
- etc..

```bash
docker run -d -p 5000:5000 --name registry -v $(pwd)/registry-data:/var/lib/registry registry

```



# Docker con 17

env top

logs stdin/stdout togther  , volume to one place.

dont save information inside container use volume.

use specific version in FROM

leave defaults config 



reason to run couple swarm

geographic boundaries 

bad reasons

diffrent subnets secutriy groups 

diffrent aviliablity 





# Kubernetes

Kubernetes usally called `k8s` as meaning of k + 8 letters + s or `kube`.

Kubernetes = or `k8s` popular container orchestrator.

Container Orchestration = Make many servers act like one.

devolved by Google now is open source.

it's runs on docker,

gives you API / CLI to manage containers across servers.



top popular way to use k8s is through cloud vendor , that provide k8s as a service.

many vendor make k8s custom we'll call it distro of it.

like Linux when we have kernel and many distro upon it  , we have the foundational of Linux and then each distro add his unique



we have it as well in kube when we have many "distro" for : cloud , on-prem,dc ,etc...



Servers + Change rate = Benefit of orchestration.

k8s is hybrid you can , run it on different places

k8s distros : 

- cloud
- self management
  - Docker Enterprise.
  - Rancher.
  - OpenShift.
  - Canocial.
  - VMWare.
  - PKS.



## k8s vs swarm :

they both built upon docker run time

swarm is easier to deploy manage

k8s more features and flexibility



swarm advantage :

- docker cli

- come from docker company

- easier to deploy/manage yourself

- has 20% of features of k8s but solve 80% of use case

- run when docker can be run (windows , arm - embdded , dc , 32-bit)

- secure by default 



k8s  :

- clouds will deploy manage for you

- wide adoption and community 

- cover wide sets of use cases 

- this is more safe to your career



## k8s architecture 

`kubectl` = kube control = k8s CLI to configure manage cluster.

`worker node`- single server in k8s cluster.

`kubelet` - k8s agent running on each node to allow node talks to manager , it needs is local API installation this is not default in docker.

`Control Plane / Master` = is the charge of running k8s cluster (likes manager in swarm) , 

contain all things that master in charge of actually is a group of containers.

in Control Plane we can find :

- API SERVER - the way we talks to cluster.
- Scheduler - control how how and where your containers are placed on nodes in object called pods.
- Controller Manager - lookjs at the state of whole cluster , take the orders/specs and determines the diffrence between what you are asking to do and what is going on
- etcd - distributed storage for key values  , you need odd numbers.
- coreDNS - control dns

In regular node : 

- kubelet 
- kube proxy - control networking



`pod` - one or more containers running together on one node. 

this is the basic unit of deployment , we don't use container but pods.



`controller` for creating/updating pods and other objects ,validate that what's going on to k8s is what you asked to do.

we have many types of controllers :

- Deployment - control your pods at lower level.
- ReplicaSet 
- SatefulSet
- DeamonSet
- Job
- CronJob
- and much much more

`service` - network endpoint to connect a pod.

`namespaces` - filtered group of objects in cluster.

`Secrets` 

`configmaps`



## Installation

you don't have to install it , you have 2 free services to play with k8s :

- https://training.play-with-kubernetes.com/
- https://katacoda.com/



who is using linux / linux vm 

```bash
sudo snap install microk8s --classic
```

now you can acesses to k8s through `mictok8s.kubectl` recommend put alias for `kubectl`.



## Hello World K8s

we have 3 commands to create pods from kubectl :

- kubetcl run (equivalent to docker run)
- kubetcl create (create some recsources via cli or yaml)
- kubetcl apply (create/update anything via YAML)



verify k8s installed successfully 

```bash
aviad@aviad-Inspiron-7373:~$ kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:58:59Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:50:46Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}

```



let's run nginx

```bash
kubectl run my-nginx --image nginx
#pod/my-nginx created

aviad@aviad-Inspiron-7373:~$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
my-nginx   1/1     Running   0          64s

# when we use run command this is using deployment controller that creates replica set controller that create the pod.

kubectl delete pod my-nginx # to delete the pod

kubectl get all # list  
```



```bash
kubectl create deployment my-apache --image httpd

kubectl scale deploy/my-apache --replicas 2 
# or 
kubectl scale deployment my-apache --replicas 2 

aviad@aviad-Inspiron-7373:~$ kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/my-apache-65fd7bd7db-tpcl7   1/1     Running   0          112s
pod/my-apache-65fd7bd7db-zq5kn   1/1     Running   0          2m37s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   47h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-apache   2/2     2            2           2m37s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/my-apache-65fd7bd7db   2         2         2       2m37s

```



```bash
aviad@aviad-Inspiron-7373:~$ kubectl logs deployment/my-apache
Found 2 pods, using pod/my-apache-65fd7bd7db-zq5kn
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.43. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.43. Set the 'ServerName' directive globally to suppress this message
[Fri Apr 03 22:59:33.797262 2020] [mpm_event:notice] [pid 1:tid 140429150393472] AH00489: Apache/2.4.43 (Unix) configured -- resuming normal operations
[Fri Apr 03 22:59:33.800669 2020] [core:notice] [pid 1:tid 140429150393472] AH00094: Command line: 'httpd -D FOREGROUND'

# match by label , you can put to various pods same label then gots here all logs from diffrent pods.
aviad@aviad-Inspiron-7373:~$ kubectl logs -l app=my-apache

AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.42. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.42. Set the 'ServerName' directive globally to suppress this message
[Fri Apr 03 22:59:35.231518 2020] [mpm_event:notice] [pid 1:tid 139766717191296] AH00489: Apache/2.4.43 (Unix) configured -- resuming normal operations
[Fri Apr 03 22:59:35.231623 2020] [core:notice] [pid 1:tid 139766717191296] AH00094: Command line: 'httpd -D FOREGROUND'
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.43. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.1.100.43. Set the 'ServerName' directive globally to suppress this message
[Fri Apr 03 22:59:33.797262 2020] [mpm_event:notice] [pid 1:tid 140429150393472] AH00489: Apache/2.4.43 (Unix) configured -- resuming normal operations
[Fri Apr 03 22:59:33.800669 2020] [core:notice] [pid 1:tid 140429150393472] AH00094: Command line: 'httpd -D FOREGROUND'

# similar to inspect into conainer

aviad@aviad-Inspiron-7373:~$ kubectl describe pod/my-apache-65fd7bd7db-tpcl7
Name:         my-apache-65fd7bd7db-tpcl7
Namespace:    default
Priority:     0
Node:         aviad-inspiron-7373/10.100.102.9
Start Time:   Sat, 04 Apr 2020 00:55:37 +0300
Labels:       app=my-apache
              pod-template-hash=65fd7bd7db
Annotations:  <none>
Status:       Running
IP:           10.1.100.42
IPs:
  IP:           10.1.100.42
Controlled By:  ReplicaSet/my-apache-65fd7bd7db
Containers:
  httpd:
    Container ID:   containerd://07c47a20953540957481fbe6cb0345a1a52c07d6ca944605ca4b71d90cf8c499
    Image:          httpd
    Image ID:       docker.io/library/httpd@sha256:13aa010584cb3d79d66adf52444494ae5db67faa28d65a1a25e6ddc57f7c0e2a
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sat, 04 Apr 2020 01:59:35 +0300
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Sat, 04 Apr 2020 00:55:41 +0300
      Finished:     Sat, 04 Apr 2020 01:10:24 +0300
    Ready:          True
    Restart Count:  1
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8zrmn (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-8zrmn:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-8zrmn
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason             Age                 From                          Message
  ----     ------             ----                ----                          -------
  Normal   Scheduled          77m                 default-scheduler             Successfully assigned default/my-apache-65fd7bd7db-tpcl7 to aviad-inspiron-7373
  Normal   Pulling            77m                 kubelet, aviad-inspiron-7373  Pulling image "httpd"
  Normal   Pulled             77m                 kubelet, aviad-inspiron-7373  Successfully pulled image "httpd"
  Normal   Created            77m                 kubelet, aviad-inspiron-7373  Created container httpd
  Normal   Started            77m                 kubelet, aviad-inspiron-7373  Started container httpd
  Warning  MissingClusterDNS  37m (x36 over 77m)  kubelet, aviad-inspiron-7373  pod: "my-apache-65fd7bd7db-tpcl7_default(239330d1-9b47-4f81-b971-0e240a3e34b6)". kubelet does not have ClusterDNS IP configured and cannot create Pod using "ClusterFirst" policy. Falling back to "Default" policy.
  Normal   SandboxChanged     13m (x2 over 13m)   kubelet, aviad-inspiron-7373  Pod sandbox changed, it will be killed and re-created.
  Normal   Pulling            13m                 kubelet, aviad-inspiron-7373  Pulling image "httpd"
  Normal   Pulled             13m                 kubelet, aviad-inspiron-7373  Successfully pulled image "httpd"
  Normal   Created            13m                 kubelet, aviad-inspiron-7373  Created container httpd
  Normal   Started            13m                 kubelet, aviad-inspiron-7373  Started container httpd
  Warning  MissingClusterDNS  32s (x14 over 13m)  kubelet, aviad-inspiron-7373  pod: "my-apache-65fd7bd7db-tpcl7_default(239330d1-9b47-4f81-b971-0e240a3e34b6)". kubelet does not have ClusterDNS IP configured and cannot create Pod using "ClusterFirst" policy. Falling back to "Default" policy.


```



what was happened ?

- deployment controller updated to 2 replicas.
- ReplicaSet controller sets pod count to 2.
- control plane assigns node to pod.
- kubelet sees pod is needed start the container.

``` 



1.12-1.15

from version 1.15

k8s reduce the functionality of kubetcl run commands and many of the generators , deprecated all of them expect of one 

 now it's very similar in experience to docker run command.


```



# Exposing K8S Ports

remember what is it a `service` ?

is a stable adress for pod(s) , this seats on top of pods and allow 

consider a stateless image-processing backend which is running with 3 replicas. Those replicas are fungible—frontends do not care which backend they use. While the actual Pods that compose the backend set may change, the frontend clients should not need to be aware of that, nor should they need to keep track of the set of backends themselves.

The Service abstraction enables this decoupling