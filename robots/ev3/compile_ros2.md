# Installing ROS 2 on EV3 (using EV3DEV DOcker)

## Preliminaries
Sources:
- https://www.ev3dev.org/
- https://www.ev3dev.org/docs/tutorials/using-docker-to-cross-compile/
- https://github.com/ev3dev/docker-library


## Pull images
Pull EV3DEV into your system (e.g. Version buster):
```bash
docker pull ev3dev/ev3dev-buster-ev3-base
docker tag ev3dev/ev3dev-buster-ev3-base ev3
```

Start image:
```
docker run --rm -it ev3 su -l robot
```

# Installing ROS 2

## Prerequisite packages

Very important packages:
```
apt update && apt install -y curl gnupg2 lsb-release libffi-dev rustc
```


Locale settings:
```
locale  # check for UTF-8

apt update && sudo apt install -y locales
locale-gen en_US en_US.UTF-8
update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale
```
Ecosystem pacakges (Colcon and setuptools are unfortunately not supported on ARMEL):
```bash
apt install -y \
  build-essential \
  cmake \
  git \
  libbullet-dev \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-cryptography \
  wget
```
Additional packages:
```bash
apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev
```
Numerical packages:
```bash
apt install -y libeigen3-dev liblog4cxx-dev python3-numpy
```
ASIO and TinyXML2:
```bash
apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev
```
Pip packages:
```bash
pip3 install -U \
  argcomplete \
  flake8-blind-except \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order \
  flake8-quotes \
  pytest-repeat \
  colcon-common-extensions \
  vcstool \
  pytest-rerunfailures \
  pytest 
```
Install lark:
```bash
pip3 install lark 
```
## Compile ROS2
```bash
mkdir -p ./ros2_foxy/src
cd ./ros2_foxy
wget https://raw.githubusercontent.com/ros2/ros2/foxy/ros2.repos
vcs import src < ros2.repos
```
You can ignore ROS-bridge, visualization tools, rviz2:
```bash
touch src/ros2/rviz/COLCON_IGNORE && touch src/ros2/rviz/AMENT_IGNORE
touch src/ros2/demos/COLCON_IGNORE && touch src/ros2/demos/AMENT_IGNORE
touch src/ros/ros_tutorials/COLCON_IGNORE && touch src/ros/ros_tutorials/AMENT_IGNORE
touch src/ros2/ros1_bridge/COLCON_IGNORE && touch src/ros2/ros1_bridge/AMENT_IGNORE
touch src/ros-visualization/COLCON_IGNORE && touch src/ros-visualization/AMENT_IGNORE
```
You can ignore Cyclone and Connext if you go over FastRTPS:
```bash
touch src/ros2/rmw_connext/COLCON_IGNORE && touch src/ros2/rmw_connext/AMENT_IGNORE
touch src/ros2/rmw_cyclonedds/COLCON_IGNORE && touch src/ros2/rmw_cyclonedds/AMENT_IGNORE
touch src/eclipse-cyclonedds/COLCON_IGNORE && touch src/eclipse-cyclonedds/AMENT_IGNORE
```
Ignore __mimick_vendor__, it won't compile on ARMv7l:
```bash
touch src/ros2/mimick_vendor/COLCON_IGNORE && touch src/ros2/mimick_vendor/AMENT_IGNORE
```
Start compiling the workspace. In this case, ensure __--merge-install__ is used: 
```bash
colcon build --merge-install --cmake-args -DBUILD_TESTING=OFF
```
### Troubleshooting
SOmetimes CUrl does not work. Use the following parameter, to download ROS keys:
```
curl -s -k https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -
```