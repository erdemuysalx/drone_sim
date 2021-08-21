
# Introduction

This is a description of how to set up a native drone development environment installation with Ubuntu 20.04, ROS Noetic Ninjemys, and Gazebo with some additional side packages.

# Setup Drone Development Environment


## ROS Noetic Ninjemys

### Setup your sources.list

Setup your computer to accept software from packages.ros.org.

`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
    
### Setup your keys

`curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

### Installation

First, make sure your Debian package index is up-to-date:

`sudo apt update`

Now pick how much of ROS you would like to install.

* **Desktop-Full Install: (Recommended)**  : Everything in  **Desktop**  plus 2D/3D simulators and 2D/3D perception packages
    
    `sudo apt install ros-noetic-desktop-full`
               
 * **Desktop Install:** Everything in  **ROS-Base**  plus tools like  [rqt](http://wiki.ros.org/rqt)  and  [rviz](http://wiki.ros.org/rviz)
    
    `sudo apt install ros-noetic-desktop`
                
* **ROS-Base: (Bare Bones)**  ROS packaging, build, and communication libraries. No GUI tools.
    
    `sudo apt install ros-noetic-ros-base`

### Environment setup

You must source this script in every  **bash**  terminal you use ROS in.

`source /opt/ros/noetic/setup.bash`

It can be convenient to automatically source this script every time a new shell is launched. These commands will do that for you.

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc`
source ~/.bashrc
```

### Dependencies for building packages

Up to now you have installed what you need to run the core ROS packages. To create and manage your own ROS workspaces, there are various tools and requirements that are distributed separately.
To install these tool and other dependencies for building ROS packages, run:

 `sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential`

#### Initialize rosdep

Before you can use many ROS tools, you will need to initialize  rosdep.  rosdep  enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. If you have not yet installed  rosdep, do so as follows.

With the following, you can initialize  rosdep.

```
sudo rosdep init
rosdep update
```

## Gazebo

Setup your computer to accept software from packages.osrfoundation.org:

```sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'```

### Setup keys

`wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -`

### Reload package list and install Gazebo
 
```
sudo apt-get update
sudo apt-get install gazebo11 libgazebo11-dev
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
```

## Ardupilot SITL & MAVProxy

### Clone ArduPilot repository
```
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
```

### Install some required packages:
`Tools/environment_install/install-prereqs-ubuntu.sh -y`

Reload the path:
`. ~/.profile`

### Start SITL simulator
```
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```

## Gazebo Ardupilot Plugin

```
git clone https://github.com/khancyr/ardupilot_gazebo
cd ardupilot_gazebo
mkdir build
cd build
cmake ..
make -j4
sudo make install
```

```
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
. ~/.bashrc
```

## Run Gazebo Simulator & Ardupilot SITL

**In terminal 1**, run Gazebo:
`gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world`

**In terminal 2**, run SITL:
```
cd ~/ardupilot/ArduCopter/
sim_vehicle.py -v ArduCopter -f gazebo-iris --console
```
