:toc: macro
toc::[]

= Docker

Introduction

== Install Docker on Windows

If you do not use Windows 10 you can not run Docker directly on your machine. Rather you need a virtual machine with a specific Linux distribution on it. Docker offers a ready to use https://www.docker.com/products/docker-toolbox[toolbox] that automates this process. In order to be able to run a virtual machine your machine needs virtualization enabled. This can be done in your BIOS settings. For a Lenovo T450 this works as follows. This will work similar for other models.

- shut down your computer
- turn on your computer and keep hitting the button F1
- you will hit some kind of beep and the BIOS will pop up
- navigate to Security, then Virtualization and hit enter
- enable Intel (R) Virtualization Technology and hit F10 and confirm the changes with Y
- boot your machine

Now install Docker Toolbox (full installation is recommended). When the installation is done start Docker Quickstart Terminal to see if everything works. As soon as you see some whale and a command prompt you can try `docker run hello-world`. This will download and run a container from docker hub and print some message if this was successful.

== Use Docker manually

As docker (boot2docker) runs on a virtualized machine you may need to share folders with this virtual machine (VM). Start Oracle VM VirtualBox. You should see a machine called default. If you started the Docker Quickstart Terminal prior to this _default_ should be running. 
 In order to add a shared folder you have to shutdown the machine. You can do this with right click on _default_, _close_, _power off_. Then click on _default_ and go to _Settings_ , _Shared Folders_. On the right you can click on _Adds new shared folder_. In folder path go to _Other_ and navigate to the folder on your machine that you wish to share with the VM (e.g. c:\dockershare) and type a name (e.g. dockershare). Toggle _Auto-mount_ and _Make Permanent_ and save the changes.
Now you can either start the VM out of Oracle VM VirtualBox or again start the Docker Quickstart Terminal. I prefer the second option as you keep your keyboard layout in the language of the host machine. If you have chosen to start the Quickstart Terminal ssh into the VM with `docker-machine ssh default`. From within the Linux VM you can execute docker commands as docker is installed on that distribution.
Now create a folder which contains the shared files and mount the share folder:

....
sudo mkdir /dockershare
sudo mount -t vboxsf dockershare /dockershare
cd /dockershare/
....

Now if there are files inside this folder on your Windows host, you can see them with `ls`. To manually create a container with the sample application, copy a bootable `oasp4j-sample-server.war` into the shared folder. Docker uses `Dockerfile` s to create images which can be started as containers. So in your shared folder create a file called `Dockerfile` (no ending) with the following content:

....
FROM java:openjdk-7-jre
COPY /oasp4j-sample-server.war ../oasp4j-sample-server.war
EXPOSE 8080
....

This will use a java 7 image as a base which gets downloaded from the public Docker repository and copy the application into the root folder of the VM. When the container is started, Docker exposes the port 8080 towards the VM. Now go back to the Quickstart Terminal. In the shared folder you can type `docker build –t java7 .` (don't forget the full stop) to build an image with the above `Dockerfile`. You can now run the container with `docker run -v /dev/urandom:/dev/random –p 8080:8080 java7 java –jar oasp-sample-server.war`. The `–p 8080:8080` maps the port of the _boot2docker_ to the port 8080 of the VM. The command `-v /dev/urandom:/dev/random` specifies where to get random numbers. Utilizing urandom drastically speeds up the boot process of a container in a docker environment. When the application is started you can connect with the IP of the VM. You can find out the IP of the VM by typing `docker-machine ip` in a console of your host machine.

In my case this is 192.168.99.100, so I can access the sample app in a browser of the Windows host with 192.168.99.100:8080. To list all running containers type `docker ps`. In order to stop a running container one can use `docker stop nameOfContainer`.

== Fabric8io plugin

Various maven plugins enable dockerziation within the process of building the application. In the devon sample server this is shown using the https://github.com/fabric8io/docker-maven-plugin[fabric8io/docker-maven-plugin] integrated as an extra profile called _docker_. Executing `mvn -Pdocker install`

- executes the build process,
- grabs the bootable war file,
- builds an image based on the configuration in the `server/pom.xml` and the `server/src/main/docker/assembly.xml`,
- deploys the image depending on the environment variables e.g. to a local instance of docker,
- starts the application, tests if it is reachable through an URL and finally 
- stops the container.

In detail, if you want to test this on your local machine you need to have docker installed as mentioned above. In a console type `docker-machine env` to see the IP and other properties of your VM. To set the environment variables so that the plugin deploys the application into your local docker instance type the last line of that output (something like `eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env)`). Now navigate to your workspace where the project is located. In the `devon/sample` folder you can now execute `mvn -Pdocker install` and see how the process starts. If you only want to build the container, you can navigate into `/devon/sample/server` and execute `mvn -Pdocker package docker:build` or start the container with `mvn -Pdocker docker:start`.