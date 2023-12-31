# humble_noetic_rosbridge_docker

Step 1: Launch the container
```
make up
make enter
```

Step 2: Terminal A - Run roscore in one container
```
source [ros1]
roscore
```

Step 3: Terminal B - Source ros2_ws/install/setup.bash
```
source ros2_ws/install/setup.bash
```

Step 4: Terminal B - Build the ros2 workspace with the ros1_bridge package
```
# Takes more than 5 minutes
colon build
```
OR
```
# Takes more than 5 minutes
colcon build --symlink-install --packages-select ros1_bridge --cmake-force-configure
```

Step 5: Terminal B - Launch rosbridge 
```
# First source ROS1
source ${ROS1_INSTALL_PATH}/setup.bash

# Source ROS2
source ${ROS2_INSTALL_PATH}/setup.bash

# Bridge all topics both ways
ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```
 
For help 
```
ros2 run ros1_bridge dynamic_bridge --help
```

