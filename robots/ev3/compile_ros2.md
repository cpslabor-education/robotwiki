# Installing ROS 2 on EV3 (using EV3DEV DOcker)

Debian version: buster

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
docker run --rm -it -v "$(pwd):/home" ev3 su -l robot
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
  libeigen3-dev \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-cryptography \
  python3-numpy \
  python3-netifaces \
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
Install ROS toolchain related tools:
```bash
pip3 install -U \
  colcon-common-extensions \
  vcstool
```
Install lark:
```bash
pip3 install lark 
```
Install additional libraries:
```bash
apt install liblog4cxx-dev
```
Install Eigen from source:
```bash
mkdir /dev && cd /dev
wget https://gitlab.com/libeigen/eigen/-/archive/3.2.10/eigen-3.2.10.zip
unzip eigen-3.2.10.zip
cd eigen-3.2.10.zip
mkdir build && cd build
cmake ..
make install
```
## Compile ROS2
Don't forget to s
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
colcon build --merge-install --cmake-args -DBUILD_TESTING=OFF -DCMAKE_SHARED_LINKER_FLAGS='-latomic' -DCMAKE_EXE_LINKE_FLAGS='-latomic'
```
Or if you want to see full output
```bash
colcon build --merge-install --cmake-args -DBUILD_TESTING=OFF -DCMAKE_SHARED_LINKER_FLAGS='-latomic' -DCMAKE_EXE_LINKE_FLAGS='-latomic' --event-handlers console_direct+
```
### Packages required on the robot
Ensure you preserve links during copying this build of ROS 2.

Install Python3 and the following packages as a prerequsite:
```bash
sudo apt install --no-install-recommends -y \
  python3 \
  libasio-dev \
  libtinyxml2-dev \
  python3-netifaces \
  python3-numpy \
  python3-yaml \
```
Ensure the binaries in the ROS 2 installation folder has execuion permissions.

### Optional Python3 pip installation
Optionally install _python3 pip_ if you want to install some additional Python 3 packages:
```
sudo apt install python3-pip
```
Optionally install the following Python packages on the robot (probably not necessary as long as it is not used for development):
```bash
sudo -H pip3 install -U \
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
  pytest-rerunfailures \
  pytest  
```

### Troubleshooting
Sometimes CUrl does not work. Use the following parameter, to download ROS keys:
```
curl -s -k https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -
```


#### Sensors not detected
If you have installed Buster version 2020-04-10, you may not detect any plugged in sensor. This issue is known, and referenced here: https://github.com/ev3dev/ev3dev/issues/1433

The authors of this page can confirm, that after dist-upgrade, the EV3 can detect sensors. This upgrades __connman__ as well, so expect connection loss during installation:
```
sudo apt-get update && sudo apt-get dist-upgrade
```
