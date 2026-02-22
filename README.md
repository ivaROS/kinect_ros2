# System `kinect_ros2`

This branch is for using the Ubuntu system packages for working with `freenect` to interface the
Kinect v1 RGBD camera.  For Ubuntu 22.04 (and most likely later), the freenect libraries have
downloadable packages.  However, the placement of the development files differs from the custom
compile and install placement.  The system version is also missing the needed CMake specification; 
solutions are to custom add a libfreenect.cmake file to `/usr/share/...` or to modify the CMakeLists file.
Either way, there are enough differences to require a separate branch.

Provides basic Kinect-v1 (for the Xbox 360) node with IPC support for a single Kinect device. 
If multiple devices present, the first one listed by the `freenect_num_devices` will be selected.

## Installing

**1. Get repo.**
Go to target ROS2 workspace and clone the repo into the `src` folder:
~~~
git clone -b system https://github.com/ivaROS/kinect_ros2
~~~

**2. Dependencies.**
Install any missing ROS packages via `rosdep`.  From within the top of the workspace, use `rosdep`
to install missing ROS2 dependencies.
~~~
rosdep install --from-paths src --ignore-src -r -y
~~~

**3. Build.**
Either build the workspace or custom build the new package via `colcon`. 
~~~
colcon build
~~~
or
~~~
colcon --packages-select kinect_ros2
~~~

## Interface

To get the ROS2 topics published,
~~~
ros2 run kinect_ros2 kinect_node
~~~
assuming that the top-level name was not changed.

### Published topics
* `~image_raw` - RGB image(rgb8) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~camera_info` - RGB camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))
* `~depth/image_raw` - Depth camera image(mono16) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~depth/camera_info` - Depth camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))

Go to target ROS2 workspace and clone the repo into the `src` folder:
~~~
git clone -b system https://github.com/ivaROS/kinect_ros2
~~~

**2. Dependencies.**
Install any missing ROS packages via `rosdep`.  From within the top of the workspace, use `rosdep`
to install missing ROS2 dependencies.
~~~
rosdep install --from-paths src --ignore-src -r -y
~~~

**3. Build.**
Either build the workspace or custom build the new package via `colcon`. 
~~~
colcon build
~~~
or
~~~
colcon --packages-select kinect_ros2
~~~

## Using this package

## Devices tested
* Kinect Model 1473
