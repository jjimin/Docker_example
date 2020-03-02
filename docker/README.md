# How to build and run a docker

In the same directory as a dockerfile, build a docker image.

 ```
 docker build --tag pytorch/pytorch:1.3-custom -f Dockerfile-pytorch-1.3 .
 ```
 
And, run a docker container.
 
 ```
 docker run -it --rm -p 2000:2000 --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
 -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY \
 --name pytorch-docker \
 pytorch/pytorch:1.3-custom \
 /bin/bash
 ```
