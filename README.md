# Docker_example
practicing docker tutorial

## Installation
### How to install 'Docker-ce'
* [description link](https://www.quantumdl.com/entry/PyTorchTensorflow%EB%A5%BC-%EC%9C%84%ED%95%9C-Docker-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
* [how to solve permission error (1)](http://www.kwangsiklee.com/2017/05/%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/)
* [how to solve permission error (2)](https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/)

 ```
 sudo apt update
 sudo apt install apt-transport-https ca-certificates curl software-properties-common
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 # Ubuntu 16.04
 sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
 # Ubuntu 18.04
 sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
 sudo apt update
 apt-cache policy docker-ce
 sudo apt install docker-ce
 ### temperary solution
 sudo chmod 666 /var/run/docker.sock
 ### permanent solution
 sudo usermod -a -G docker $USER
 sudo service docker restart
 ```
 
### How to install nvidia-docker to use nvidia setting in dockers
 ```
 # in a host terminal
 distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
 curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
 curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
 sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
 sudo systemctl restart docker
 docker run --rm --gpus all nvidia/cuda:10.1-base nvidia-smi
 ```
 
 
### How to install 'pyTorch 1.3 docker image'
* [description link](https://www.quantumdl.com/entry/PyTorchTensorflow%EB%A5%BC-%EC%9C%84%ED%95%9C-Docker-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
* [docker hub](https://hub.docker.com/)
* [pytorch 1.3 docker image](https://hub.docker.com/r/pytorch/pytorch/tags?page=1&name=1.3)

 ```
 docker pull pytorch/pytorch:1.3-cuda10.1-cudnn7-devel
 ```
 
 ### How to install 'sublime-text'
  ```
  # Install Sublime Text
  sudo apt-get update
  wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
  echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
  sudo apt-get update
  sudo apt-get install -y sublime-text
  ```
  
### Example of docker run command
 ```
 # (1)
 docker run -it --rm -p 2000-2002:2000-2002 \
 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 --name ros-carla-01 \
 carla-ros-bridge:custom-01 \
 /bin/bash
 
 # (2)
 docker run -it --rm -p 2000:2000 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 --name pytorch-docker \
 pytorch/pytorch:1.3-cuda10.1-cudnn7-devel \
 /bin/bash
 
 # (3)
 docker run -it --rm -p 2000:2000 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 --name pytorch-docker \
 pytorch/pytorch:1.3-custom \
 /bin/bash
 
 # (4)
 docker run -it -p 2000:2000 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 -v $(pwd):/data \
 --name pytorch-docker \
 pytorch/pytorch:1.3-custom \
 /bin/bash
 
 # (5)
 docker run -it -p 2000:2000 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 -v $(pwd)/home/pytorch-home:/home/torch -v $(pwd)/dataset/data_odometry_velodyne:/data \
 --name pytorch-docker-01 \
 jjimin/pytorch:1.3-5.0 \
 /bin/bash

 ```

### Delete docker <none> image

```
 docker rmi $(docker images -f "dangling=true" -q)
 ```

