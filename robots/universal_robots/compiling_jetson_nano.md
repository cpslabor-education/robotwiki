# Compiling Universal Robots ROS2 driver

## Preliminaries
It is recommended to check the folowing document, where the steps to compile ROS2 are discussed: [Steps to use Docker for cross-compilation on ARM64](../../docker/cross_compilation_debian.md)

## Install prerequistie packages
For installation, consult the following repository: https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver

You will need _console-bridge_ libraries:
```bash
apt install libconsole-bridge-dev
```

You will need the following additional packages (besides UR-ROS2):
```
git clone https://github.com/ros/angles -b ros2 ./src/angles
git clone https://github.com/ros-controls/realtime_tools.git -b ros2_devel ./src/realtime_tools
```

## Error log

### Package: joint_state_broadcaster


### Upgrade to Python 3

Source: https://www.itsupportwale.com/blog/how-to-upgrade-to-python-3-8-on-ubuntu-18-04-lts/

