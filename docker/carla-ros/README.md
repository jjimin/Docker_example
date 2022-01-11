# Quick start of CARLA-ROS docker image
how to use CARLA-ROS docker

## How to create CARLA-ROS container

Select a tag from one of the versions below.
#### list of carla-ros versions
* `1.0` (No longer in use)
* `2.0` (No longer in use)
* `2.1` (No longer in use)
* `16.04-kinetic-0.9.8_v1.0`
   * ubuntu 16.04
   * carla 0.9.8
   * ros kinetic
   * carla-ros-bridge 0.9.8
* `18.04-foxy-0.9.11_v1.0`
   * ubuntu 18.04
   * carla 0.9.11
   * ros foxy
   * carla-ros-bridge 0.9.11
* `20.04-foxy-0.9.11_v0.5` (dependencies broken)
   * ubuntu 20.04
   * carla 0.9.11
   * ros foxy
   * carla-ros-bridge 0.9.11
* `20.04-foxy-0.9.13_v1.0` (**recommended**)
   * ubuntu 20.04
   * carla 0.9.13
   * ros foxy
   * carla-ros-bridge 0.9.11

Download the public image from docker hub.
```
docker pull jjimin/carla-ros:<tag_name>
```

Create a container with the downloaded image. (There are some options for using GPU and display.)
```
docker run -it --rm \
--privileged --net=host --gpus all -e NVIDIA_VISIBLE_DEVICES=0 \
-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY --name <container_name> \
jjimin/carla-ros:<tag_name> /bin/bash
```

## How to run server and bridge in CARLA-ROS container
* [description link](https://carla.readthedocs.io/projects/ros-bridge/en/latest/ros_installation_ros2/#run-the-ros-bridge)


Use alias command to run the carla server.
```
server-run-onscreen # Use 'server-run-offscreen' for off-screen mode
```

In another terminal, start the bridge for ROS2. You can run one of the two options below: 
```
# Option 1, start the basic ROS bridge package
ros2 launch carla_ros_bridge carla_ros_bridge.launch.py
```
```
# Option 2, start the ROS bridge with an example ego vehicle
ros2 launch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch.py
```
