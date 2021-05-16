# Installing ROS 2 Java

## Preliminaries

### Required packages
Install the following packages:
```bash
sudo apt install libasio-dev
```

## Installation instructions
TODO: add TL;dr
```bash
mkdir -p ~/your_ros2_java_workspace/src
cd ~/your_ros2_java_workspace
curl -skL https://raw.githubusercontent.com/esteve/ament_java/master/ament_java.repos -o ament_java.repos
vcs import src < ament_java.repos
src/ament/ament_tools/scripts/ament.py build --symlink-install --isolated
source install_isolated/local_setup.bash
curl -skL https://raw.githubusercontent.com/esteve/ros2_java/master/ros2_java_desktop.repos -o ros2_java_desktop.repos
vcs import src < ros2_java_desktop.repos
ament build --symlink-install --isolated
```