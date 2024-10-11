![Apache 2.0 License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)

# Moveit2 Jazzy Ros2 workspace

This is a ROS 2 Jazzy workspace that brings together all the ```jazzy``` packages for Moveit2 software stack. It can then be readily used with Gazebo, Open 3D Engine software simulation engines or any other ROS 2 software that depends on moveit2. 

**TODO** how long it takes to build, disk space, recommended RAM

## Setup 

**TODO** add instructions: Requries python3-vcs tools, add instruction for installing it

* Download this repo and download packages defined in ```moveit2_jazzy.repos``` file.
```bash
cd ~
git clone https://github.com/Mechazo11/moveit2_jazzy_ws
vcs import src < moveit2_jazzy.repos
rosdep install -r --from-paths src --rosdistro jazzy -i -y
```

* Either source your global ROS 2 Jazzy workspace or build a ROS2 Jazzy workspace from source. See this [example](https://github.com/Mechazo11/ubuntu22_jazzy_ws) for more details. Then build the workspace

```bash
source ~/ubuntu22_jazzy_ws/install/setup.bash
colcon build --event-handlers desktop_notification- status- --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## Misc

* Generate a package list for moveit2

```bash
cd ~/Downloads
git clone https://github.com/moveit/moveit2.git -b main
cd moveit2/
rosinstall_generator \
  --rosdistro jazzy \
  --deps -- \
  --exclude-path ~/ubuntu22_jazzy_ws/src \
  --exclude $(cat excluded-pkgs.txt) -- \
  -- $(cat moveit2-pkgs.txt) \
  > moveit2_generated_pkgs.repos
```