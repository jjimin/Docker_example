# Quick start of CARLA-ROS docker image
how to use CARLA-ROS docker

## Installation
### How to install
```
# download the image
docker pull jjimin/carla-ros:<tag_name>

# create a container with the image
docker run -it --rm --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
jjimin/carla-ros:<tag_name> /bin/bash
```
