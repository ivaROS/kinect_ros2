# Custom `kinect_ros2`

This branch is for using a custom compiled `freenect` to interface the Kinect v1 RGBD camera.  
It ignored the Ubuntu 22.04 (and higher) freenect packages.  The system files are sufficiently
different from the custom compiled ones that a different branch is required.

Provides basic Kinect-v1 (for the Xbox 360) node with IPC support for a single Kinect device,
based on [libfreenect](https://github.com/OpenKinect/libfreenect).
If multiple devices are present, the first one listed by the `freenect_num_devices` will be selected.

## Installing

### Install libfreenect
Obtain the public [libfreenect](https://github.com/OpenKinect/libfreenect) github repo, build it,
and install.  The default install prefix is going to be `/usr/local`, which means that some system
edits are needed.  The `/etc/environment` specification needs to add `usr/local/bin` if it doesn't
have it already (should).  

The linker path listing needs to include the location of the installed libraries.   Normally that
would be done through a file in the `/etc/ld.so.conf.d/` directory.
If the proper `libc` packages are installed, then `libc.conf` will exist and point to
`/usr/local/lib`.  Check that it is so.  If not, then add a `local.conf` file to do so:

> sudo echo "/usr/local/lib" > /etc/ld.so.conf.d/local.conf

## Interface

To get the ROS2 topics published,
~~~
ros2 run kinect kinect_node
~~~

### Published topics
* `~image_raw` - RGB image(rgb8) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~camera_info` - RGB camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))
* `~depth/image_raw` - Depth camera image(mono16) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~depth/camera_info` - Depth camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))

## Instalation

### 2. Copy the repo
Copy the repo to your workspace source folder.
~~~
cd ~/ws/src
git clone https://github.com/fadlio/kinect_ros2
~~~

### 3. Install any missing ROS packages
Use `rosdep` from the top directory of your workspace to install any missing ROS related dependency.
~~~
cd ~/ws
rosdep install --from-paths src --ignore-src -r -y
~~~

### 4. Build your workspace
From the top directory of your workspace, use `colcon` to build your packages.
~~~
cd ~/ws
colcon build
~~~

## Using this package

## Devices tested
* Kinect Model 1473
