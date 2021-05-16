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

