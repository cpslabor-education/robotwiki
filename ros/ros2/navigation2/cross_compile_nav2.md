# Cross compilation of Nav2

Sources:
- https://navigation.ros.org/build_instructions/index.html

## Prerequisites

### Packages
Install the following packages (GraphicsMagick, a ZeroMQ for BehaviorTree):
```bash
apt install -y libgraphicsmagick-dev graphicsmagick-libmagick-dev-compat
apt install -y libceres-dev
apt install -y 
```
ZMQ On Ubuntu 20.04 and Debian:
```bash
apt install -y libzmq-dev
```
ZMQ on Jetson Nano (Ubuntu 18.04 Bionic):
```bash
apt install -y libzmq3-dev
```

### Repositories to pull
Clone the following repositories into your workspace of choice:
```bash
git clone https://github.com/ros-planning/navigation2.git
git clone https://github.com/ros/bond_core.git -b dashing-devel # They use dashing-devel for development for Foxy...
git clone https://github.com/BehaviorTree/BehaviorTree.CPP.git
git clone https://github.com/ros/angles.git -b ros2
```

## Compile Nav2
If you intend to use it on Jetson Nano, use the following:
```
colcon build --merge-install
```

If you don't want to use RVIZ and other visualization plugins (e.g. because you're on Jetson Nano), you can use the following command to ignore these pacakges:
```
touch src/navigation2/nav2_rviz_plugins/AMENT_IGNORE && touch src/navigation2/nav2_rviz_plugins/COLCON_IGNORE
```
You can ignore system tests also in this case:
```bash
touch src/navigation2/nav2_system_tests/AMENT_IGNORE && touch src/navigation2/nav2_system_tests/COLCON_IGNORE
```