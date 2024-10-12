![Apache 2.0 License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)

# Moveit2 ROS2 Jazzy Workspace

This is a ROS 2 Jazzy workspace that brings together all the ```jazzy``` packages for Moveit2 software stack. It can then be readily used with Gazebo, Open 3D Engine software simulation engines or any other ROS 2 software that depends on moveit2. 

**NOTE**, could not reconsile STOMP and thus I am skipping building the STOMP planner.

**TODO** how long it takes to build, disk space, recommended RAM

## Setup 

**TODO** add instructions: Requries python3-vcs tools, add instruction for installing it

* Install dependencies

```bash
sudo apt update
sudo apt upgrade
sudo apt-get install libompl-dev
```

* Download this repo and download packages defined in ```moveit2_jazzy.repos``` file.
```bash
cd ~
git clone https://github.com/Mechazo11/moveit2_jazzy_ws
cd moveit2_jazzy_ws
vcs import src < moveit2_jazzy.repos --recursive
rosdep install -r --from-paths src --rosdistro jazzy -i -y
```

* Either source your global ROS 2 Jazzy workspace or build a ROS2 Jazzy workspace from source. See this [example](https://github.com/Mechazo11/ubuntu22_jazzy_ws) for more details. Then build the workspace

```bash
source ~/ubuntu22_jazzy_ws/install/setup.bash
colcon build --packages-up-to moveit_planners_ompl --packages-ignore stomp stomp_moveit moveit_planners_stomp --cmake-args -DCMAKE_BUILD_TYPE=Release
source ./install/setup.bash
colcon build --packages-ignore stomp stomp_moveit moveit_planners_stomp --cmake-args -DCMAKE_BUILD_TYPE=Release
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

```
colcon build --packages-select angles eigen_stl_containers moveit_common moveit_resources_panda_description moveit_resources_pr2_description octomap_msgs osqp_vendor random_numbers srdfdom geometric_shapes moveit_resources_panda_moveit_config object_recognition_msgs moveit_msgs


```

```
To suppress this warning ignore these packages in the workspace:
--packages-ignore ament_cmake_core ament_cmake_export_definitions ament_cmake_export_include_directories ament_cmake_export_libraries ament_cmake_export_link_flags ament_cmake_include_directories ament_cmake_libraries ament_cmake_python ament_cmake_version ament_cmake_export_dependencies ament_cmake_export_interfaces ament_cmake_export_targets ament_cmake_target_dependencies ament_cmake_test ament_cmake_gtest ament_cmake_gen_version_h ament_cmake action_msgs
```


```bash
--- stderr: moveit_core                                          
In file included from /home/tigerwife/moveit2_jazzy_ws/install/ruckig/include/ruckig/calculator.hpp:5,
                 from /home/tigerwife/moveit2_jazzy_ws/install/ruckig/include/ruckig/ruckig.hpp:13,
                 from /home/tigerwife/moveit2_jazzy_ws/src/moveit/moveit2/moveit_core/online_signal_smoothing/include/moveit/online_signal_smoothing/ruckig_filter.h:47,
                 from /home/tigerwife/moveit2_jazzy_ws/src/moveit/moveit2/moveit_core/online_signal_smoothing/src/ruckig_filter.cpp:35:
/home/tigerwife/moveit2_jazzy_ws/install/ruckig/include/ruckig/calculator_cloud.hpp:12:10: fatal error: nlohmann/json.hpp: No such file or directory
   12 | #include <nlohmann/json.hpp>
      |          ^~~~~~~~~~~~~~~~~~~
compilation terminated.
gmake[2]: *** [online_signal_smoothing/CMakeFiles/moveit_ruckig_filter.dir/build.make:76: online_signal_smoothing/CMakeFiles/moveit_ruckig_filter.dir/src/ruckig_filter.cpp.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:1887: online_signal_smoothing/CMakeFiles/moveit_ruckig_filter.dir/all] Error 2
gmake[1]: *** Waiting for unfinished jobs....
gmake: *** [Makefile:146: all] Error 2
---
```

```bash
'ament_cmake_pytest' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_pytest, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_pytest
        'ament_cmake' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake
        'ament_cmake_test' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_test, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_test
        'ament_cmake_google_benchmark' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_google_benchmark, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_google_benchmark
        'action_msgs' is in: /home/icore/moveit2_jazzy_ws/install/action_msgs, /home/icore/ubuntu22_jazzy_ws/install/action_msgs
        'ament_cmake_ros' is in: /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_ros
        'ament_cmake_gtest' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_gtest, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_gtest
        'ament_cmake_core' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_core, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_core
        'ament_cmake_gmock' is in: /home/icore/moveit2_jazzy_ws/install/ament_cmake_gmock, /home/icore/ubuntu22_jazzy_ws/install/ament_cmake_gmock
```