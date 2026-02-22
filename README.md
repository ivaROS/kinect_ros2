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

~~~
sudo echo "/usr/local/lib" > /etc/ld.so.conf.d/local.conf
sudo ldconfig
~~~

### Install ROS2 package

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

## Interface and Usage

To get the ROS2 topics published,
~~~
ros2 run kinect_ros2 kinect_node
~~~

assuming that the top-level name was not changed.
It should not have been since such an edit would be a bit more massive and affect the code.
However, eventually it should be since the idea of tacking `ros2` onto the name makes no sense
given that the `ros2` command is being used.  It is redundant.

### Published topics
* `~image_raw` - RGB image(rgb8) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~camera_info` - RGB camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))
* `~depth/image_raw` - Depth camera image(mono16) ([sensor_msgs/Image](http://docs.ros.org/api/sensor_msgs/html/msg/Image.html))
* `~depth/camera_info` - Depth camera_info ([sensor_msgs/CameraInfo](http://docs.ros.org/api/sensor_msgs/html/msg/CameraInfo.html))

### Confirming functionality

Sample viewers are provided to confirm the implementation.  One for the color image:
~~~
ros2 launch kinect_ros2 showimage.launch.py
~~~
One for the depth image as remapped to a point cloud:
~~~
ros2 launch kinect_ros2 pointcloud.launch.py
~~~

### Devices tested
* Kinect Model 1473
