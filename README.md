# ROS Melodic - VNC-enabled Docker container
Docker container featuring a full ROS Melodic Ubuntu Bionic installation. Quick and easy way to set up ROS in an isolated environment, with convenient HTML5 and VNC to access the desktop environment. Heavily based on [bpinaya's project](https://github.com/bpinaya/robond-docker).

## Docker Setup
  - Installation's instructions for Ubuntu can be found [HERE](https://docs.docker.com/engine/installation/linux/ubuntu/)
  - Installation's instructions for Windows can be found [HERE](https://docs.docker.com/docker-for-windows/install/)
  - Installation's instructions for MacOS can be found [HERE](https://docs.docker.com/docker-for-mac/install/)

## Running the container
After having installed Docker, you can pull the image and run the container with
```
docker run -it --rm karolorlamowski/ros-desktop-full-vnc:melodic
```
This will pull the image from the repo and start it as a container. You will be logged as root in the container console and will see some debug messages.

![alt text](img/readme1.png)

For a quick sanity check, you can run in another terminal

```
docker exec -it `docker ps -l -q` bash
```
This will open a terminal console in the container, letting you type in whatever command you might need. Try launching `roscore` or any command you would expect to work in a ROS installation. If the ros master spins up, everything seems in order.

## Running in browser
You can make use of the HTML5 interface by launching the container with a specific port
```
docker run -it --rm -p 6080:80 karolorlamowski/ros-desktop-full-vnc:melodic
```
and then you can access to it by navigating to
```
localhost:6080
```

## Running in VNC
If you want to use a VNC client (Like [RealVNC](https://www.realvnc.com/download/viewer/)), go with
```
docker run -it --rm -p 6080:80 -p 5900:5900 karolorlamowski/ros-desktop-full-vnc:melodic
```
Then open it in your VNC viewer with the port 5900.

![alt text](img/readme2.png)

## Share files with the container
The image comes with a catkin workspace already set up in `/home/ubuntu/ros_wsp`. You can write and pull packages in the container, however __keep in mind that any change will be gone when you kill the container__. Make sure to push your changes (either with `docker commit`, or on any external software repo e.g. github) before you kill the container.

A fast and easy way to retain any change in your catkin workspace is to mount it as a volume in the host operating system. For instance, let's say you use Ubuntu and your catkin workspace is in `/home/karolorlamowski/catkin_ws`, you can run the container with the following command

```
docker run -it --rm -p 6080:80 -p 5900:5900 -v $HOME/workspace:/home/ubuntu/ros_ws karolorlamowski/ros-desktop-full-vnc:melodic
```

You can populate the directory as you wish from the host system, and the packages will show up in the container workspace (and vice versa!).

## Acknowledgements
  - This image is based on [FCWU image](https://github.com/fcwu/docker-ubuntu-vnc-desktop) , that has the support for the VNC server with browser support, so no VNC client is needed, kudos to him!
  - Part of the readme come from [bpinaya](https://github.com/bpinaya/robond-docker)'s project.
