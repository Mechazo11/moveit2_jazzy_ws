![Apache 2.0 License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)

# Moveit2 ROS2 Jazzy Workspace

This is a ROS 2 Jazzy workspace that brings together all the ```jazzy``` packages for Moveit2 and Nav2 software stacks. It can then be readily used with Gazebo Harmonic, Open 3D Engine software simulation engines or any other downstream software stack that depends on moveit2 and nav2. Must have access to a base ROS 2 Jazzy workspace, either installed from binaries or built from source such as this [one]()

**NOTE**, could not reconsile STOMP and thus I am skipping building the STOMP planner.


## Setup 

**TODO** add instructions: Requries python3-vcs tools, add instruction for installing it

* Install dependencies

```bash
sudo apt update
sudo apt upgrade
sudo apt-get install libompl-dev libyaml-cpp-dev
```

* Download this repo and download packages defined in ```moveit2_jazzy.repos``` file.
```bash
cd ~
git clone https://github.com/Mechazo11/moveit2_nav2_jazzy_ws.git
cd moveit2_nav2_jazzy_ws
vcs import src < moveit2_nav2_jazzy.repos --recursive
rosdep install -r --from-paths src --rosdistro jazzy -i -y
```

* Either source your global ROS 2 Jazzy workspace or build a ROS2 Jazzy workspace from source. See this [example](https://github.com/Mechazo11/ubuntu22_jazzy_ws) for more details. Then build the workspace

```bash
source ~/ubuntu22_jazzy_ws/install/setup.bash
colcon build --packages-up-to moveit_planners_ompl --packages-ignore stomp stomp_moveit moveit_planners_stomp --cmake-args -DCMAKE_BUILD_TYPE=Release
source ./install/setup.bash
#colcon build --packages-ignore stomp stomp_moveit moveit_planners_stomp --cmake-args -DCMAKE_BUILD_TYPE=Release
colcon build --packages-ignore stomp stomp_moveit moveit_planners_stomp --cmake-args -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-w"
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

* Open issue for nav2_amcl build
```bash
In file included from /home/tigerwife/moveit2_nav2_jazzy_ws/src/ros-navigation/navigation2/nav2_amcl/include/nav2_amcl/amcl_node.hpp:32,
                 from /home/tigerwife/moveit2_nav2_jazzy_ws/src/ros-navigation/navigation2/nav2_amcl/src/amcl_node.cpp:23:
/home/tigerwife/moveit2_nav2_jazzy_ws/install/message_filters/include/message_filters/message_filters/subscriber.h:18:2: error: #warning This header is obsolete, please include message_filters/subscriber.hpp instead [-Werror=cpp]
   18 | #warning This header is obsolete, please include message_filters/subscriber.hpp instead
      |  ^~~~~~~
In file included from /home/tigerwife/ubuntu22_jazzy_ws/install/tf2_ros/include/tf2_ros/tf2_ros/message_filter.h:50,
                 from /home/tigerwife/moveit2_nav2_jazzy_ws/src/ros-navigation/navigation2/nav2_amcl/include/nav2_amcl/amcl_node.hpp:49:
/home/tigerwife/moveit2_nav2_jazzy_ws/install/message_filters/include/message_filters/message_filters/connection.h:18:2: error: #warning This header is obsolete, please include message_filters/connection.hpp instead [-Werror=cpp]
   18 | #warning This header is obsolete, please include message_filters/connection.hpp instead
      |  ^~~~~~~
In file included from /home/tigerwife/ubuntu22_jazzy_ws/install/tf2_ros/include/tf2_ros/tf2_ros/message_filter.h:51:
/home/tigerwife/moveit2_nav2_jazzy_ws/install/message_filters/include/message_filters/message_filters/message_traits.h:18:2: error: #warning This header is obsolete, please include message_filters/message_traits.hpp instead [-Werror=cpp]
   18 | #warning This header is obsolete, please include message_filters/message_traits.hpp instead
      |  ^~~~~~~
In file included from /home/tigerwife/ubuntu22_jazzy_ws/install/tf2_ros/include/tf2_ros/tf2_ros/message_filter.h:52:
/home/tigerwife/moveit2_nav2_jazzy_ws/install/message_filters/include/message_filters/message_filters/simple_filter.h:18:2: error: #warning This header is obsolete, please include message_filters/simple_filter.hpp instead [-Werror=cpp]
   18 | #warning This header is obsolete, please include message_filters/simple_filter.hpp instead
      |  ^~~~~~~
cc1plus: all warnings being treated as errors
gmake[2]: *** [CMakeFiles/amcl_core.dir/build.make:76: CMakeFiles/amcl_core.dir/src/amcl_node.cpp.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:241: CMakeFiles/amcl_core.dir/all] Error 2
gmake: *** [Makefile:146: all] Error 2
```

* Error with nav2_waypoint_follower

```bash
--- stderr: nav2_waypoint_follower                                                             
CMake Error at /home/tigerwife/moveit2_nav2_jazzy_ws/install/robot_localization/share/robot_localization/cmake/robot_localizationExport.cmake:61 (set_target_properties):
  The link interface of target "robot_localization::rl_lib" contains:

    yaml-cpp::yaml-cpp

  but the target was not found.  Possible reasons include:

    * There is a typo in the target name.
    * A find_package call is missing for an IMPORTED target.
    * An ALIAS target is missing.

Call Stack (most recent call first):
  /home/tigerwife/moveit2_nav2_jazzy_ws/install/robot_localization/share/robot_localization/cmake/ament_cmake_export_targets-extras.cmake:9 (include)
  /home/tigerwife/moveit2_nav2_jazzy_ws/install/robot_localization/share/robot_localization/cmake/robot_localizationConfig.cmake:41 (include)
  CMakeLists.txt:20 (find_package)
```