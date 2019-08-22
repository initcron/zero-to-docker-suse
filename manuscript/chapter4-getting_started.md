# Getting Started with Docker

In this chapter, you are going to learn about docker shell, the command line utility and how to use it to launch containers. You will also learn what it means to run a container, its lifecycle and perform basic operations such as creating, starting, stopping, removing, pausing containers and checking the status etc.


### Using docker cli  
We can use docker cli to interact with docker daemon. Various functions of docker command is given below. Try this yourself by runnig **$sudo docker** command  

```
docker
```  

[Output]  
```
Usage: docker [OPTIONS] COMMAND [arg...]
       docker [ --help | -v | --version ]

A self-sufficient runtime for containers.

Options:

  --config=~/.docker              Location of client config files
  -D, --debug                     Enable debug mode
  -H, --host=[]                   Daemon socket(s) to connect to
  -h, --help                      Print usage
  -l, --log-level=info            Set the logging level
  --tls                           Use TLS; implied by --tlsverify
  --tlscacert=~/.docker/ca.pem    Trust certs signed only by this CA
  --tlscert=~/.docker/cert.pem    Path to TLS certificate file
  --tlskey=~/.docker/key.pem      Path to TLS key file
  --tlsverify                     Use TLS and verify the remote
  -v, --version                   Print version information and quit

Commands:
    attach    Attach to a running container
    build     Build an image from a Dockerfile
    commit    Create a new image from a container's changes
    cp        Copy files/folders between a container and the local filesystem
    create    Create a new container
    diff      Inspect changes on a container's filesystem
    events    Get real time events from the server
    exec      Run a command in a running container
    export    Export a container's filesystem as a tar archive
    history   Show the history of an image
    images    List images
    import    Import the contents from a tarball to create a filesystem image
    info      Display system-wide information
    inspect   Return low-level information on a container, image or task
    kill      Kill one or more running containers
    load      Load an image from a tar archive or STDIN
    login     Log in to a Docker registry.
    logout    Log out from a Docker registry.
    logs      Fetch the logs of a container
    network   Manage Docker networks
    node      Manage Docker Swarm nodes
    pause     Pause all processes within one or more containers
    port      List port mappings or a specific mapping for the container
    ps        List containers
    pull      Pull an image or a repository from a registry
    push      Push an image or a repository to a registry
    rename    Rename a container
    restart   Restart a container
    rm        Remove one or more containers
    rmi       Remove one or more images
    run       Run a command in a new container
    save      Save one or more images to a tar archive (streamed to STDOUT by default)
    search    Search the Docker Hub for images
    service   Manage Docker services
    start     Start one or more stopped containers
    stats     Display a live stream of container(s) resource usage statistics
    stop      Stop one or more running containers
    swarm     Manage Docker Swarm
    tag       Tag an image into a repository
    top       Display the running processes of a container
    unpause   Unpause all processes within one or more containers
    update    Update configuration of one or more containers
    version   Show the Docker version information
    volume    Manage Docker volumes
    wait      Block until a container stops, then print its exit code

```

#### Getting Information about Docker Setup  
We can get the information about our Docker setup in several ways. Namely,  

```
docker -v  

docker version  

docker system info
```  

[Output of **docker -v**]  

```
Docker version 18.03.1-ce, build 9ee9f40
```  

[Output of **docker version**]   

```
Client:
 Version:      18.03.1-ce
 API version:  1.37
 Go version:   go1.9.5
 Git commit:   9ee9f40
 Built:        Thu Apr 26 07:18:46 2018
 OS/Arch:      linux/amd64
 Experimental: false
 Orchestrator: swarm

Server:
 Engine:
  Version:      18.03.1-ce
  API version:  1.37 (minimum version 1.12)
  Go version:   go1.9.5
  Git commit:   9ee9f40
  Built:        Thu Apr 26 07:16:59 2018
  OS/Arch:      linux/amd64
  Experimental: false
```  


The **docker system info** command gives a lot of useful information like total number of containers and images along with information about host resource utilization  etc.


### Launching our first container    

Now we have a basic understanding of docker command and sub commands, let us dive straight into launching our very first **container**  

```
docker run alpine:3.4 uptime
```  
Where,
  * we are using docker **client** to
  * run a application/command **uptime** using  
  * an image by name **alpine:3.4**

[Output]  
```
Unable to find image 'alpine:3.4' locally
3.4: Pulling from library/alpine
81033e7c1d6a: Pull complete
Digest: sha256:2532609239f3a96fbc30670716d87c8861b8a1974564211325633ca093b11c0b
Status: Downloaded newer image for alpine:3.4

 15:24:34 up  7:36,  load average: 0.00, 0.03, 0.04
```  

**What happened?**  

This command will  
  * Pull the **alpine** image file from **docker hub**, a cloud registry
  * Create a runtime environment/ container with the above image   
  * Launch a program (called **uptime**) inside that container  
  * Stream that output to the terminal  
  * Stop the container once the program is exited

**Where did my container go?**  

```
docker   ps

docker   ps -l
```

The point here to remember is that, when that executable stops running inside the container, the container itself will stop  
This process will further be explained under the **lifecycle of a container** topic.


Let's see what happens when we run that command again,  

[Output]  
```
docker run alpine uptime
 07:48:06 up  3:15,  load average: 0.00, 0.00, 0.00
```  

Now docker no longer pulls the image again from registry, because **it has stored the image locally** from the previous run  
So once an image is pulled, we can make use of that image to create and run as many container as we want without the need of downloading the image again. However it has created a new instance of the iamge/container.

Try running a few more containers

```
docker run alpine:3.4 free

docker run alpine:3.4 uname -a

docker run alpine:3.4 ps aux

```



### Checking Status of the containers  

We have understood how docker run commands works. But what if you want to see list of running containers and history of containers that had run and exited? This can be done by executing the following commands  

```
docker ps
```  

[Output]  

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```  
This command doesn't give us any information. Because, **docker ps** command will only show list of container(s) which are **running**  

```
docker ps -l
```  

[Output]  
```
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES
988f4d90d604        alpine              "uptime"            About a minute ago   Exited (0) About a minute ago                       fervent_hypatia
```  
the **-l** flag shows the last run container along with other details like image it used, command it executed, return code of that command, etc.,  

```
docker ps -n 2
```  
[Output]  
```
NAMES
988f4d90d604        alpine              "uptime"            About a minute ago   Exited (0) About a minute ago                       fervent_hypatia
acea3023dca4        alpine              "uptime"            3 minutes ago        Exited (0) 3 minutes ago                            mad_darwin
```  
Docker gives us the flexibility to show the desirable number of last run containers. This can be achieved by using **-n #no_of_results** flag  

```
docker ps -a
```  

[Output]  

```
CONTAINER ID        IMAGE                        COMMAND                  CREATED              STATUS                          PORTS                  NAMES
988f4d90d604        alpine                       "uptime"                 About a minute ago   Exited (0) About a minute ago                          fervent_hypatia
acea3023dca4        alpine                       "uptime"                 4 minutes ago        Exited (0) 4 minutes ago                               mad_darwin
60ffa94e69ec        ubuntu:14.04.3               "bash"                   27 hours ago         Exited (0) 26 hours ago                                infallible_meninsky
dd75c04e7d2b        schoolofdevops/ghost:0.3.1   "/entrypoint.sh npm s"   4 days ago           Exited (0) 3 days ago                                  kickass_bardeen
c082972f66d6        schoolofdevops/ghost:0.3.1   "/entrypoint.sh npm s"   4 days ago           Exited (0) 3 days ago           0.0.0.0:80->2368/tcp   sodcblog

```  
This command will show all the container we have run so far.  

### Running Containers in Interactive Mode


We can interact with docker containers by giving -it flags at the run time. These flags stand for  

  * i - Interactive  
  * t - tty

```
docker run -it opensuse/leap:15 bash
```  

[Output]  

```
Unable to find image 'opensuse/leap:15' locally
15: Pulling from opensuse/leap
2242934cd532: Pull complete
Digest: sha256:50145e974549cec61181861ba86c831aeb4fc8ce7ee74dfd424cd39a029e7320
Status: Downloaded newer image for opensuse/leap:15
309d534d077d:/ #
```  

As you see, we have landed straight into **bash** shell of that container. This is the result of using **-it** flags and mentioning that container to run the **bash** shell. Don't try to exit that container yet. We have to execute some other commands in it to understand the next topic  


**Checking whats namespaced:**

Like a full fledged OS, Docker container has its own namespaces  
This enables Docker container to isolate itself from the host as well as other containers  

Run the following commands and see that alpine container has its own namespaces and not inheriting much from **host OS**  

```
cat /etc/issue

ps aux

zypper install -y net-tools-deprecated
ifconfig

hostname
```  


**Checking whats not namespaced:**  

We have understood that containers have their own namespaces. But will they share something to some extent? the answer is **YES**. Let's run the following commands on both the container and the host machine  

[Command]  
```
uptime

uname -a

date  

cat /proc/cpuinfo


```  


Now exit out of that container by running **exit** or by pressing **ctrl+d**  


### Making Containers Persist  

#### Running Containers in Detached Mode  
So far, we have run the containers interactively. But this is not always the case. Sometimes you may want to start a container  without interacting with it. This can be achieved by using **"detached mode"** (**-d**) flag. Hence the container will launch the deafault application inside and run in the background. This saves a lot of time, we don't have to wait till the applications launches successfully. It will happen behind the screen. Let us run the following command to see this in action  

[Command]  

```
docker run -idt schoolofdevops/loop program
```  

-d , --detach : detached mode  

[Output]  

```
2533adf280ac4838b446e4ee1151f52793e6ac499d2e631b2c752459bb18ad5f
```  
This will run the container in detached mode. We are only given with full container id as the output  

Let us check whether this container is running or not  
[Command]  
```
docker ps
```  

[Output]  

```
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS              PORTS               NAMES
2533adf280ac        schoolofdevops/loop   "program"           37 seconds ago      Up 36 seconds                           prickly_bose
```  
As we can see in the output, the container is running in the background  


#### Checking Logs

To check the logs, find out the container id/name and run the following commands, replacing 08f0242aa61c with your container id


[Commands]  
```
docker container ps

docker container logs 08f0242aa61c

docker container logs -f  08f0242aa61c
```  



#### Connecting to running container to execute commands
We can connect to the containers which are running in detached mode by using these following commands  
[Command]  
```
docker exec -it 2533adf280ac sh
```  

[Output]  

```
/ #
```  

You could try running any commands on the shell
e.g.

```
apk update
apk add vim
ps aux
```

Now exit the container.  

#### Pausing Running Container  
Just like in a video, it is easy to pause and unpause the running container  
[Command]  

```
docker pause 2533adf280ac
```  

After running pause command, run docker ps again to check the container status  

[Output]  

```
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS                  PORTS               NAMES
2533adf280ac        schoolofdevops/loop   "program"           2 minutes ago       Up 2 minutes (Paused)                       prickly_bose
```  

#### Unpausing the paused container  
This can be achieved by executing following command  

[Command]  
```
docker unpause
```  

Run docker ps to verify the changes  

[Output]  

```
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS              PORTS               NAMES
2533adf280ac        schoolofdevops/loop   "program"           6 minutes ago       Up 6 minutes                            prickly_bose
```  

### Launching a Suse Linux 15 container

Use the following commands to launch and learn how to work with SLES15 container 
```

docker pull registry.suse.com/suse/sle15

docker image ls

docker container run -idt --name sles15 registry.suse.com/suse/sle15 bash

docker exec -it sles15 bash

[try running  suse linux  commands]

exit
```

### Stopping and Removing Containers
We have learnt about interacting with a container, running a container, pausing and unpausing a container, creating and starting a container. But what if you want to stop the container or remove the container itself  

#### Stop a container  
A container can be stopped using **stop** command. This command will stop the application inside that container hence the container itself will be stopped. This command basically sends a **SIGTERM** signal to the container (graceful shutdown)  

[Command]  

```
docker stop 2533adf280ac
```  
[Output]  

```
2533adf280ac
```  
#### Kill a container  
This command will send **SIGKILL** signal and kills the container ungracefully  

[Command]  

```
docker kill 590e7060743a
```  

[Output]  

```
590e7060743a
```  
If you want to remove a container, then execute the following command. Before running this command, run docker ps -a to see the list of pre run containers. Choose a container of your wish and then execute docker rm command. Then run docker ps -a again to check the removed container list or not  

[Command]  

```
docker rm 590e7060743a
```  

[Output]  

```
590e7060743a
```  
