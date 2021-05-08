# Install simualted PixHawk (Gazebo) with ROS2

## Abbreviations
- SITL: Software in the loop simulation
- HITL: Hardwaer in the loop simulation

## Sources
The following tutorials have been used:
- https://docs.px4.io/master/en/simulation/gazebo.html
- https://docs.px4.io/master/en/ros/ros2_comm.html
- https://docs.px4.io/master/en/middleware/micrortps.html
- https://docs.px4.io/master/en/dev_setup/dev_env_linux_ubuntu.html#gazebo-jmavsim-and-nuttx-pixhawk-targets

## Installation
The following steps have been tested on Ubuntu 20.04. Please don't use any kind of Ubuntu under 18.04.

### FastRTPSGen
FastRTPSGen is required, which is a generator tool for Micro-DDS and FastDDS. To install this tool you shall need:
- JDK 11 (14.0.2 tested, broken as of 2021 May 07). You can switch between Java versions with the following command (in each step select the most appropriate version of JDK 11):
```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
- Gradle (recommended to be installed with SDKMAN: https://sdkman.io/)
It is recommended to build it from source rather than installing from binaries. Install it by pulling the source and install with gradle:
```bash
git clone --recursive https://github.com/eProsima/Fast-RTPS-Gen.git -b v1.0.4 ~/Fast-RTPS-Gen \
    && cd ~/Fast-RTPS-Gen \
    && gradle assemble \
    && gradle install
```

Or if you fail at installation, ensure you have super user rights:
```bash
sudo gradle install
```

You can check if everything worked out normally

Source: https://dev.px4.io/v1.11_noredirect/en/setup/fast-rtps-installation.html

It is assumed that with ROS2 installation you already have Fast-DDS installed, so you can ignore installing that from source.

### QGroundControl
For basic teleoperation and control, QGroundControl (http://qgroundcontrol.com/) is a recommnneded tool. It's a binary application that can be easily installed on any system (Windows, MacOS, Linux also supported).

### Install Gazebo simulator
The beneficial thing with PX4 is that the simulator and the toolchain comes hand-in-hand. You can easily switch between simulated and real platform. Also SITL means, that you get a NuttX emulator as well, to further enhance software integration.

First, pull the PX4 target sources to your workspace of choice:
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```
Then start installation (you can safely install simulation alongside existing ROS2-Gazebo installation), which install NuttX development tools & simulator alongside PX4 toolset:
```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

### Start simulation
After the installation steps, you can build the simulation:
```bash
make px4_sitl gazebo
```
After successful build, Gazebo will automatically start. Everytime you want to start simulation, you can safely reissue the command above.

If you want to use RTPS interface, issue the following command:
```bash
make px4_sitl_rtps gazebo
```

You can interact with the Drone through the NuttX console. To start the MicroRTPS client on the system issue the following command:
```bash
micrortps_client start -t UDP
```

Takeoff drone:
```
commander takeoff
```

## ROS 2 interface

### Install ROS 2 packages
Install ROS 2 by pulling the following pacakges to your workspace:
```bash
git clone https://github.com/PX4/px4_ros_com.git ~/px4_ros_com_ros2/src/px4_ros_com
git clone https://github.com/PX4/px4_ros_com.git ~/px4_ros_com_ros2/src/px4_ros_com
```
Use the script provided by the developers to install:
```bash
cd scripts
source build_ros2_workspace.bash
```


### Start ROS 2 wrapper
To communicate with the PX4, start the following commands (use the setup file provided by the workspace):
```bash
source ~/px4_ros_com_ros2/install/setup.bash
micrortps_agent -t UDP
```

Ensure the MicroRTPS client has been started on the target:
```bash
micrortps_client start -t UDP
```

Then start the RTPS listener:
```bash
source ~/px4_ros_com_ros2/install/setup.bash
ros2 launch px4_ros_com sensor_combined_listener.launch.py
```

Ensure you have data, by checking topic frequency of arbitrary topic:
```bash
ros2 topic hz /SensorCombined_PubSubTopic
```
