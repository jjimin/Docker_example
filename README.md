# Docker_example
how to install and show examples of using various docker images

## Installation
### How to install 'Docker-ce'
* [description link (1)](https://docs.docker.com/engine/install/ubuntu/)
* [description link (2)](https://www.quantumdl.com/entry/PyTorchTensorflow%EB%A5%BC-%EC%9C%84%ED%95%9C-Docker-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
   - [what are differences between CE and EE](https://nobase-dev.tistory.com/34) (CE: community edition, EE: enterprise edition)
 
 Install docker
 ```
 sudo apt update
 sudo apt install apt-transport-https ca-certificates curl software-properties-common
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
 sudo apt update
 apt-cache policy docker-ce
 sudo apt install docker-ce docker-ce-cli containerd.io 
 
 ```
 Check the installation
 ```
 docker --version
 ```
 
 Change setting to use docker without 'sudo'
 
 * [how to solve permission error (1)](http://www.kwangsiklee.com/2017/05/%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/)
* [how to solve permission error (2)](https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/)
 ```
 ### temperary solution
 sudo chmod 666 /var/run/docker.sock
 
 ### permanent solution
 sudo usermod -a -G docker $USER
 sudo service docker restart
 # After this, please restart your computer.
 ```
 
### How to install nvidia-docker to use nvidia setting in dockers
* [description link (1)](https://github.com/NVIDIA/nvidia-docker)
* [description link (2)](https://www.quantumdl.com/entry/PyTorchTensorflow%EB%A5%BC-%EC%9C%84%ED%95%9C-Docker-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
* [description link (3)](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker)
 ```
 distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
 curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
 curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
 sudo apt-get update
 # sudo apt-get install nvidia-container-toolkit
 sudo apt-get install nvidia-docker2
 sudo systemctl restart docker
 
 ### check the installation
 docker run --rm --gpus all nvidia/cuda:10.1-base nvidia-smi
 ```

## Command Practice and Tips for using docker
### Commands
* [easy tutorial with docker (1)](https://tecadmin.net/install-docker-on-ubuntu/)
* [easy tutorial with docker (2)](http://pyrasis.com/Docker/Docker-HOWTO)
#### docker images
* check installed images
 ```
 docker images
 ```
#### docker pull
* download images from [docker hub](https://hub.docker.com/)
 ```
 docker search ubuntu
 docker pull ubuntu  # default tag: latest
 ## OR other version of ubuntu
 docker pull ubuntu:xenial
 ```
#### docker ps
* check existing containers
 ```
 docker ps -a
 ```
* check only running containers
 ```
 docker ps
 ```
#### docker run
* launch new container with image
 ```
 # docker run <options> <image> <command>
 docker run -it --rm ubuntu /bin/bash
 
 # in another host terminal
 docker ps -a
 docker ps
 
 docker run -it --name hello ubuntu /bin/bash
 ```
#### docker start/stop/attach/restart
* docker can be run in back/foreground
 ```
 # docker start/stop/attach/restart <container>
 docker start hello   ## if you run this command, you can run docker container in background
 docker attach hello  ## connecting standard input(stdin) and standard output(stdout) to a running container
 ```
#### docker exec
* it can execute a command inside a container from outside
* typically used to connect another terminal to a container
 ```
 docker exec -it hello /bin/bash
 ```
#### docker inspect
* check container information (ip, port, etc.)
 ```
 # docker inspect <container>
 docker inspect hello
 ```
#### docker cp
* Copy file to/from container
 ```
 touch hello.txt
 # docker cp <container>:<docker-path> <host-path>
 # docker cp <host-path> <container>:<docker-path>
 docker cp ./hello.txt hello:/home
 docker cp hello:/home/hello.txt ../
 ```
#### docker commit
* create an image of changes made to a container
 ```
 # docker commit <container> <repository>/<image>:<tag>
 docker commit hello jjimin/ubuntu:1.0
 ```
### Tips
#### how to delete 'none' docker image
```
docker rmi $(docker images -f "dangling=true" -q)
```

#### how to use `sudo` and added user
```
# example with ROS image
# Shell (1)
docker run --rm --user root --name <container-name> ros:foxy-ros-base /bin/bash
# inside of container
apt install sudo
useradd --user-group --system --create-home --no-log-init --shell /bin/bash <user-name>
echo "<user-name>:<user-name>" | chpasswd && adduser <user-name> sudo
```
```
# Shell (2)
# outside of container
docker commit <container-name> <new-image-name>
```
```
# Shell (1)
# inside of container
exit
# outside of container
docker run --rm --user <user-name> --workdir /home/<user-name> --name <container-name> <new-image-name> /bin/bash
```
```
# Shell (2)
# outside of container
docker commit <container-name> <new-image-name>
```

### how to install VS-Code
```
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt update
sudo apt install code
```


## Examples of using various docker images

### How to install 'ROS docker image'
* [description link](http://wiki.ros.org/docker/Tutorials/Docker)
* [ROS docker image](https://registry.hub.docker.com/_/ros/?tab=tags)
 ```
 docker pull ros:kinetic-ros-base-xenial
 ```
 
### How to install 'pyTorch 1.3 docker image'
* [description link](https://www.quantumdl.com/entry/PyTorchTensorflow%EB%A5%BC-%EC%9C%84%ED%95%9C-Docker-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
* [pytorch 1.3 docker image](https://hub.docker.com/r/pytorch/pytorch/tags?page=1&name=1.3)
 ```
 docker pull pytorch/pytorch:1.3-cuda10.1-cudnn7-devel
 ```
  
### How to install and use 'CARLA-ROS docker image'
* [CARLA-ROS docker image](https://hub.docker.com/r/jjimin/carla-ros/tags)
 ```
 # download the image
 docker pull jjimin/carla-ros:18.04-foxy-0.9.11_v1.0
 
 # create a container with the image
 docker run -it --rm --privileged --net=host --runtime=nvidia --gpus all -e NVIDIA_VISIBLE_DEVICES=0 -e NVIDIA_DRIVER_CAPABILITIES=all -e QT_X11_NO_MITSHM=1 -v /tmp/.X11-unix:/tmp/.X11-unix -v /dev/shm:/dev/shm -e DISPLAY=unix$DISPLAY \
 -v ~/Data:/data --name test-01 jjimin/carla-ros:18.04-foxy-0.9.11_v1.0 /bin/bash
 ```

