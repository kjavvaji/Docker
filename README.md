# Docker

Docker is one of the technology in Devops.</br>

Docker is a platform for developing , shipping and running applications using container virtualization technology.</br>

**Docker container as a service (CAAS)::**</br>
Docker separates the applications from infrastructure using container technology,</br>
similar to how virtual machines separate the operating system from the bare metal.</br>

**Virtual Machines::**</br>
Each virtualized application includes not only the  application which may be only 10’s of MB and the necessary binaries and </br>
libraries but also an entire operating system which may weigh 10’s of GB.

**Docker::**</br>
The docker  container comprises just the application and its dependencies.It run as an isolated process in user space on the </br>
host operating system. Thus it enjoys the resource isolation and allocation benefits of VM’s but is much more portable and efficient.

**Docker benefits::**
```
  Scalable   : easily scale up or down can be done.
  portable   : by making use of snap shots.
  Deployment : is very easy.
  Density    : is very high.because of resource isolation, we can have more number of application
               running on same operating system.
```
**Docker Core components::**
```
 1) Docker Daemon:
   	Docker engine runs on the host machine.
        We will spin up Containers inside the Docker Daemon.
 2) Docker Client:
	CLI used to interact with the Daemon.
```   
Building Docker on non-linux kernel is somewhat different when compared to building docker on linux kernel.The Docker daemon uses the Linux kernel, so it isn’t possible to run Docker natively on a non-Linux PC. To get around this, the Docker team created a helper tool called Boot2Docker.
**Boot2Docker::**
The Boot2Docker installation package contains a VirtualBox virtual machine, the Boot2Docker tool, and Docker itself. The virtual machine included in the package is a lightweight Linux VirtualBox image that provides all the Linux kernel related features required by the Docker daemon.</br>
In effect, what happens is:</br>
the Linux VM runs on top of your native OS and the Docker daemon uses the Linux VM for all its Linux kernel dependencies.

**Docker Workflow components::**
```
Docker image     : Holds the environment and your application.
Docker Container : Created from images,Start,Stop,Move,Delete
Docker registry  :  public or private repositories used to store images.
Dockerfile       : Automates image construction.
```
If a container is  created from the image and if any changes made to image then containers will automatically bring those changes to them.</br>
There is a docker hub which is used to store docker images.</br>
From docker registry, we can push or pull images to our local server.</br>
upon run on these images. we can build containers.</br>
upon commit , if any changes are made in the images, those changes will be replicated in the containers. </br> 
we can also tag images like version-1 and version-2 images. </br>
Using build, we can automate the image construction using Dockerfile. </br>

**Installing Docker engine on ubuntu::**</br>
Type the following command.
```
wget -qO- https://get.docker.com/ | sh
wget options:
  q tells wget to operate quietly i.e not output the usual status information.
  O- tells send it output to /dev/null
```
2) For testing the installation use the following command.
 ```
 sudo docker run hello-world
 ```
If everything is installed correctly, it will print the following output.
```
Hello from Docker.
```
This message shows that your installation appears to be working correctly.
If needed , start the service docker...using the following command.
```
sudo service docker start
```
There will be a group created in the name of “daemon”
if we added current user to this daemon group then we don’t need to run the docker commands  using sudo.
without sudo we can run the docker commands.
let us say the current user is ubuntu.
use the following command to add ubuntu user to daemon group.
```
sudo chmod -aG daemon root
Option explanation::
    -G means new list of supplementary groups means the groups that are created with third party softwares just like the daemon group.
```
check these groups in file /etc/passwd
-a means append the user to the “Supplemental groups mentioned by the -G option without removing him/her from other groups.

Docker has client server architecture.
client take user inputs and sends them to the daemon. where daemon is the server running on some host.
Daemon builds,runs and distribute containers.
Client and Daemon can run on the same host or on different hosts.
we can use either
   1) CLI Client 
   2) GUI Client (Kitematic)	

```docker version``` command will give the both client and server(Daemon) versions.


**Docker Images and Containers::**</br>
**Images::**</br>
      Read only template used to create containers.These may be built by us or some other docker users. These images are stored in Docker Hub or our local registry.
Docker Hub is a Public Registry.
**Containers::**</br>
      These are isolated application platform.It contains everything needed to run your application.
Containers may be based on one or more images.

**Docker Orchestration::**</br>
Three tools for orchestrating distributed applications with Docker.
1) Docker machine : 
     Tool that provisions Docker hosts and installs the docker engine on them.
2) Docker swarm:
     Tool that clusters many engines and schedule containers.
3)  Docker compose:
      Tool to create and manage multi-container applications.

**Displaying local images::**</br>

we use the following command to display the local images
```
docker images
```
When creating a container, always it will attempt to use a local image first. 
If no local image is found, the Docker daemon will look in Docker Hub unless another registry is specified.

**Image Tags::**</br>
images are specified by repository:tag
The same image may have multiple tags. The default tag is the latest.

**Creating a Container::**</br>
we use ```docker run``` command.
Syntax::::
```
sudo docker run [options] [image] [command] [arguments]
```
Image is specified with repository:tag
Examples:

```docker run ubuntu:14.04 echo “Hello world"```

for first time run of this command, it will take more time because it has to download that image from public repository.
from next time, the time  taken for this command is very less since this image will found locally.

Next type the following command.
```docker run ubuntu ps```
ps is the command we supplied to docker run.
It will have a process id of 1
***********************************
The command we supplied at the time of docker run will alway have a process id of **ONE** 
**How to run a container with terminal::**</br>
we need to use -i and -t flags with docker run.
The Flag -i tells docker to connect to STDIN on the container.
The Flag -t specifies to get a Pseudo-terminal.

we need to run a terminal process as our command like /bin/bash
```sudo docker run -i -t ubuntu:latest /bin/bash```

then type ps command to view the process id’s
bash will have a process id of ONE.
Type the following commands in container::
```adduser vinodh
   adduser vinodh sudo  # adding vinodh to sudo group.
   su vinodh
   sudo apt-get install vim
   vim test..py
   exit    # to come out of vinodh account.
   exit # to come out of the container account.
```
once we type exit in terminal of a container,it will shut down the container.
since it will kill the bash process.

Now run the command as above and try to switch to vinodh user.
```
sudo docker run -i -t ubuntu:latest /bin/bash
su vinodh
```
it will give error saying that ```No passwd entry for user ‘vinodh’```
try open /etc/passwd file , we can not find any user with name vinodh.
The conclusion from here is,
it will start the new container.


A container only runs as long as the process from your specified docker run command is running.This command’s process is always PID 1 inside the container.

We can exit from the container by using CTRL+P+Q

again run the container using this.
sudo docker run -it ubuntu bash
to start the bash shell of a container.

**Container ID::**
     Containers can be specified using their ID or name.
There will be long ID and short ID
Short ID and name can be obtained using ```docker ps``` command to list the containers.
Long ID is obtained by inspecting the container.


use ```docker ps``` to list running containers.
use the flag ```-a``` to list all containers including the containers that are stopped.


**Running in Detached mode::**
Also known as running in the background or as a daemon mode.
use flag ```-d``` to run in the detached mode.
To observe output use the following command.
```docker logs [container id]```
container id can be seen from the following command.
```docker ps ```
example:
```docker run -d centos:7 ping 127.0.0.1 -c 200```
now type ```docker ps```
if ping action is not completed within this time, then docker ps command will show the centos container.
if ping action is completed by the time you run docker ps command then this command will not show the current container.
it will be visible using ```docker ps -a``` command.
To observer the output use the following command.
```docker logs container_id```
here container_id is the short name that is visible after you run ```docker ps``` command.
To view the process that is going on , use the following command.
```docker logs -f container id```

**Running a web application in a container::**
The -P flag is used to map container ports to host ports.

Create a container using a tomcat image, run in detached mode and map the tomcat ports to the host port.
```
docker run -d -P tomcat:7
```
ubuntu:trusty docker image will contains all the default commands that will come
with the ubuntu operating system.

There are two methods for creating the Docker images.

1) Docker Commit Approach

     a)  Launch a container from any existing image.
     
     b)  Make the required changes in the running container by executing commands inside the container.
     
     c)  Then commit that container with a new tag.
     
2) Dockerfile

      Gives an automated way of making images in our build and deployment pipeline.

## Dockerfile

    Dockerfile contains the step by step instructions to create a docker image with our own requirements.
   
Using Dockerfile, we can create the docker images during build time in jenkins and can push the images to docker 

registry(public or private) and these images can be used further to create the docker container by pulling them 

from docker registry.

The command for creating the docker images from the Dockerfile is 
```shell
 docker build -t user_name/image_name:tag build_context
 ```
the above prescribed tag format is used when you want to push the docker images to Docker Hub.

for pushing the images to private registry, the tag format will be different.

 **user_name**     : Docker hub id which you will use to login to Docker Hub.

 **image_name**    : Name which you want to set for the image.

 **tag**           : String which you want to set for the tag.

 **build_context** :

          1) Directory in your local file system which contains a Dockerfile.
          
          2) Git repositry url which contains a Dockerfile.
          
  Examples of Docker build commands.
  
  Suppose there is directory named 
  ```shell 
  /home/ubuntu/nginx
  ```
  which contains a Dockerfile.
  
  ```shell
  cd /home/ubuntu/nginx
  docker build -t vinodhbasavani/nginx:1.0 .
  ```
  Here . refers to Current Working Directory.
  OR
  
  ```shell
  docker build -t vinodhbasavani/nginx:1.0 /home/ubuntu/nginx/
  
 ```
 
 ```shell
 docker build -t vinodhbasavani/nginx:1.0 https://github.com/vinodhbasavani/nginx.git
 ```
 
 where nginx repository contains a Dockerfile.
 
 If you are building the images multiple times, then docker daemon will use cache for faster building of the image.
 
 if you do not want to use the cache during image creation, use
 
 ```shell
 docker build --no-cache -t vinodhbasavani/nginx:1.0 .
 ```
  If you did not specify the tag, the it will create the docker image with *latest* as tag.
  
  ***Writing the Dockerfiles***
  
  Dockerfile are nothing but a set of Instructions. The instructions are written in UPPER CASE format.
  
  Examples of Instructions are 
  
  FROM, MAINTAINER, RUN, ADD, COPY, CMD, ENTRYPOINT, VOLUME, ENV, ARG, LABEL, EXPOSE, USER, WORKDIR .
  
  Instructions in the Dockerfile are executed in the defined order. Each and Every instruction in the Dockerfile adds an 
  
  additional layer to the previous image and then does a commit.
  
  **FROM**
  
  FROM instruction is used to set the BASE Image for our Docker Image(which is a CHILD image). Each and Every Dockerfile must
  
  have atleast one FROM instruction since we should have some BASE image. We can have multiple FROM instructions in one
  
  Dockerfile.
  
  FROM instruction will be in 3 different forms as below.
  
     1) FROM image_name
     2) FROM image_name:tag
     3) FROM image_name@digest
  
The first form will pull the prescribed image with latest tag.
  
The second form will pull the prescribed image with the prescribed tag.
  
The third form will pull the prescribed image with the prescribed digest.
  
Each and Every image/layer will have an image id and Digest. Layers are  identified by a digest, which takes the form
  
algorithm:hex. The hex element is calculated by applying the algorithm (SHA256) to a layer's content. If the content changes,
  
then the computed digest will also change.
  
In order to get the digest of all images use the below command.
```shell
docker images --digests
```
To get the digest of a particular image use the below command.
```shell
docker images ubuntu:latest --digests
```
**RUN**

RUN instruction is used to execute the commands while building the image.
  
  RUN has two different forms
  
   1) Exec form
   
   2) Shell form
   
RUN instruction in EXEC form will look like

     RUN ["executable","param1","param2"] 
     
RUN instruction in Shell form will look like

     RUN command 

In shell form, we can use \(backslash) to write a mutliple commands in different lines as shown below.

```shell
RUN apt-get install -y \
      apt-get install -y apache2
```
RUN command in shell form is executed as
```shell
/bin/sh -c command
```
by default.

If we want to use different shell other than **/bin/sh** we must use the Exec form as

```shell
RUN ["/bin/bash","-c","command"]
```

The Exec command does not use shell so normal shell processing will not happen.

$HOME is an environment variable in linux.

```shell
RUN ["/bin/echo","$HOME"]
```
will not give the value of the Environment variable $HOME rather it will print the $HOME as it is.

To have normal shell processing using Exec form, we should use RUN as below.
```shell
RUN ["/bin/bash","-c","echo","$HOME"]
```

**CMD**

We will use CMD instruction in the Dockerfile to specify which command should run when container is created using that image.

CMD ["/usr/sbin/nginx","-g","daemon off;"]

will execute /usr/sbin/nginx -g "daemon off;" command when we run the container as

docker run --rm -d -p 80:80 --name test_nginx vinodhbasavani/nginx:2.0

using the image vinodhbasavani/nginx:2.0


1) if we are supplying the command during docker run then this supplied command will

  override the CMD instruction command in the Dockerfile

  ```shell
  docker run --rm -d -it  -p 80:80 --name test_nginx vinodhbasavani/nginx:2.0 /bin/bash
  ```

  will start the container but nginx service will not run inside the container since that

  command is overridden by the /bin/bash command.

2) If we have multiple CMD instructions inside the Dockerfile, then only the last CMD

   instruction will be used by the docker.

3) If we want to run multiple processes inside the container then we should use some

   management tools like "Supervisor" for managing multiple processes inside the docker.

   Once supervisor is configured, we wil use CMD for supervisor so supervisor will take care of

   all required processes inside the container.

4) If we don't want to override the CMD argument with docker run command then

   we need to use **ENTRYPOINT** instead of CMD. but we can forcefully override ENTRYPOINT defined in Dockerfile
   
   during run command using **--entrypoint** flag.

with ENTRYPOINT, the command we pass during docker run is passed as additional parameters to the command specified in the

ENTRYPOINT.


The CMD strings will be appended to the ENTRYPOINT in order to generate the container's command string.

docker run --rm -d -p 80:80 --name test_nginx vinodhbasavani/nginx:4.0 "daemon off;"

The total command to start a nginx service is 
```shell
/usr/sbin/nginx -g "daemon off;"
```
since /usr/sbin/nginx -g is added in the ENTRYPOINT instruction so "daemon off;" is passed as command to docker run command.

we can have both CMD and ENTRYPOINT in the Dockerfile.


When both an ENTRYPOINT and CMD are specified, the CMD string(s) will be appended to the ENTRYPOINT in order to generate the

container's command string.

Both the ENTRYPOINT and CMD instructions support two different forms

     1) Shell form
     2) Exec form


1) Shell form:

      In this form, the instructions are like
  
      CMD executable param1 param2
      
      ENTRYPOINT executable param1 param2

When using the shell form, the specified binary is executed with an invocation of the shell using "/bin/sh -c"

Here in this case , /bin/sh executable will have a process id of 1 but our command(say ping command in the above Dockerfile) 

will not have a process id  of 1. This will be problem some times



2) Exec form


    In this form, the instructions are like
     
    CMD ["executable","param1","param2"]


when using this form, the commands will be executed without a shell.


When using ENTRYPOINT and CMD together it's important that you always use the exec form of both instructions.

Trying to use the shell form, or mixing-and-matching the shell and exec forms will almost never give you the result you want.

Below is the tabular form with different combinations of CMD and ENTRYPOINT instructions.


|s.no |  Dockerfile                          |            command
------|--------------------------------------|----------------------------------------------------------
|1    |  ENTRYPOINT /bin/ping -c 3           |
|     |  CMD localhost                       |          /bin/sh -c '/bin/ping -c 3' /bin/sh -c localhost
|2    |  ENTRYPOINT ["/bin/ping","-c","3"]   |
|     | CMD localhost                        |         /bin/ping -c 3 /bin/sh -c localhost
|3    |  ENTRYPOINT /bin/ping -c 3           |
|     |  CMD ["localhost"]                   |          /bin/sh -c '/bin/ping -c 3' localhost
|4    |  ENTRYPOINT ["/bin/ping","-c","3"]   |
|     |  CMD ["localhost"]                   |          /bin/ping -c 3 localhost


Only s.no 4 i.e using both ENTRYPOINT and CMD in exec form will give the desired result.

suppose we created a docker image as vinodhbasavani/cmdentrypoint:1.0 using the above docker image then

```shell
docker run vinodhbasavani/cmdentrypoint:1.0 
```
will ping the localhost.

```shell
docker run vinodhbasavani/cmdentrypoint:1.0 google.com
```
will ping the google.com as localhost in CMD instruction is overridden by the google.com

Suppose we created a docker image as vinodhbasavani/cmdentrypoint:2.0 with above docker file.

docker run vinodhbasavani/cmdentrypoint:2.0 

will not ping any host rather it print the help of the ping command.

docker run vinodhbasavani/cmdentrypoint:2.0 localhost 

will ping the localhost.

**Note:** 
```shell
 1) if command supplied in the #docker run command is in shell form, it must give the ERROR.
 2) if command supplied in the #docker run command is in exec form , it will give the OUTPUT.
```
so The commands which are passed to #docker run command are in "EXEC" form only.

The Main difference between RUN and CMD,ENTRYPOINT instructions is, The RUN instruction commands will be executed when docker daemon is building the docker image. where as CMD,ENTRYPOINT instructions will executed when docker daemon is starting the container from that
image.

**ENV**

This instruction is used to set environment variables right inside our Dockerfile for our containers launched using that image.

Format of ENV instruction is simple, it just accepts a key value pair.

Examples are shown below.

```shell
ENV MY_PATH /opt
or
ENV MY_PATH=/opt
```
We can pass the environment variables during docker run command with **--env** flag.

**ADD**

ADD instruction is used to add files and directories from our docker build directory to inside of our image.

The format of ADD instruction is 
```shell
ADD source destination
```
destination is the directory inside the image.

source can be 
  1) URL
  2) file
  3) Directory
  
The main requirement for source is , these should be inside our current build directory if source is either a file or directory.

```shell
ADD index.html /usr/share/nginx/html/index.html
```
```shell
ADD https://archive.apache.org/dist/httpd/binaries/linux/apache_1.3.27-x86_64-whatever-linux22.tar.gz /opt/
```
Here ADD is used for downloading a file from a URL and placing it inside the image.

The main feature of ADD is **automated unpacking of compressed files**

```shell
ADD apache-tomcat-8.0.21.tar.gz /opt/tomcat
```
This will untar the apache-tomcat-8.0.21.tar.gz to the directory inside /opt/tomcat in the container.

**The above feature will not work if the SOURCE is URL.**

If there is a file/directory in teh destination that has the same name, it will not be overwritten.

If the target destination does not exist, Docker will create it.

If the source is URL and destination does not end with a file name then file is downloaded from URL to destination with same

name as in the URL.

If the source is URL and destination does end with a file name then file is downloaded from URL to destination with the give 

name.

**COPY**

COPY is similar to the ADD but it does not have the decompression features which are availiable with the CMD instruction.

```shell
COPY conf/ /usr/share/nginx/html
```
will copy the files/directories inside the conf directory to /usr/share/nginx/html directory.

**VOLUME**

There are two different forms of VOLUME instruction.

```shell
VOLUME ["/data","/data1"]

VOLUME /data1 /data2
```
The volume instruction creates a mount point with the specified name and this instruction does not tell what to mount from local 

system. This only tells docker that whatever placed in that volume location will not be part of that image and these will be 

accessible from any other containers as well.

```
docker run -d --volumes-from data_container -p 80:80 --name app_server test_image
```

will make volumes of the container named data_container accessible to the app_server container.

we can mount the data from docker host to this volume during docker run command.

```shell
docker run -v /home/test:/data test_image
```
then whole of the content inside /home/test will be mounted to /data inside the container.

**If any build steps change the data within the volume after it has been declared, those changes will be discarded.**

**WORKDIR**
  
  This instruction is used to set the working directory for any instruction (RUN, CMD, ENTRYPOINT, COPY and ADD) 
  
  inside the Dockerfile.
 
 **USER**
 
 The USER instruction sets the user name or UID to use when running the image and for any RUN, CMD and ENTRYPOINT instructions
 
 that follow it in the Dockerfile.
 
 This user must be created before using if that user does not exist in the image other wise docker daemon will give you an error 
 
 stating that "no matching entries in passwd file".
 
 **LABEL**
 
 This instruction is used to add Metadata to an image.
 
 A LABEL is a key value pair.
 
 **ARG**
 
 This instruction is used to pass key value pairs during the image build time.
 
 ARG instruction takes the below from.
 
 ```shell
 ARG name=default_value
 or
 ARG name
 ```
 ```shell
 docker build --build-arg HOME=/home/vinodh SHELL=/bin/bash -t vinodhbasavani/test:1.0 . 
 ```
 Later in the Dockerfile, we can use the ARG name as **$name**
 
 **NOTE:**
 Since ARG apply during build time, ARG can be used only in RUN , ADD, COPY instruction as these instruction are executed during        image build time. It can not be used in either CMD or ENTRYPOINT instruction as these instructions are executed during run time(during    container creation time). ENV instructions can be used in any instructions.

    
We can use ARG or ENV instruction to specify variables that are availiable to RUN instruction. Environment variables defined

using ENV instruction always override  ARG instruction of same name.
 
 **EXPORT**
 The EXPOSE instruction tells that the container listens on the specified network ports at runtime. EXPOSE does not make the
 
 ports of the container accessible to the host. To do that, we must use either the -p flag to publish a range of ports or 
 
 the -P flag to publish all of the exposed ports.

**ONBUILD**
 
 These instructions are executed at a later time but these will not be executed during build time of the image.
 
These instructions will be executed when we are using this image as base image for building some other child images.

```shell
ONBUILD ADD . /var/www
```
ONBUILD accepts any other Docker instructions as parameters. However we must not use Maintainer, FROM and ONBUILD itself as

parameter.

