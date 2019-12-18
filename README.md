## Perception-systems---Homework-2

### Exercise 2.1.Calibrate the intrinsics parameters of your webcam.

First of all, fork and clone the following repository:https://github.com/beta-robots/usb_cam_calibration

1- Install the following packages to be able to calibrate the camera:

```sh
$ sudo apt-get install ros-melodic-camera-calibration
$ sudo apt-get install ros-melodic-usb-cam
```

2- Change the square dimension in the line *"	args="--size 8x6 --square 0.1673 image:=/usb_cam/image_raw camera:=usb_cam">"* of the file "usb_camera_calibration.launch" of the "launch" repository:


```sh
 ~/catkin_ws/src/usb_cam_calibration/launch$ gedit usb_camera_calibration.launch
```

 
 <launch>
	<!-- User arguments -->
	<arg name="video_device"  default="/dev/video0" />
	<arg name="show_image"  default="false" />

	<!-- camera capture -->
	<node
		name="usb_cam"
		pkg="usb_cam"
		type="usb_cam_node"
		output="screen" >
		<param name="video_device" value="$(arg video_device)" />
		<param name="image_width" value="640" />
		<param name="image_height" value="480" />
		<param name="pixel_format" value="yuyv" />
		<param name="camera_frame_id" value="usb_cam" />
		<param name="io_method" value="mmap"/>
	</node>

	<!-- camera calibration -->
	<node
		name="camera_calibration"
		pkg="camera_calibration"
		type="cameracalibrator.py"
		output="screen"
		args="--size 8x6 --square 0.1673 image:=/usb_cam/image_raw camera:=usb_cam">
	</node>

	<!-- display raw image -->
	<group if="$(arg show_image)">
		<node
			name="image_view"
			pkg="image_view"
			type="image_view"
			respawn="false"
			output="screen">
			<remap from="image" to="/usb_cam/image_raw"/>
			<param name="autosize" value="true" />
		</node>
	</group>

</launch>

3- Execute the calibration program:

```sh
~/catkin_ws/src$ roslaunch usb_cam_calibration usb_camera_calibration.launch video_device:="/dev/video0"
```

.. image:: ../Perception-systems---Homework-2/master/Calibration_program.png
  :alt:
  :align: center

4- Compare the rectified image with real one:


```sh
$ roslaunch usb_cam_calibration usb_camera_calibrated.launch video_device:="/dev/video0"
```

### Exercise 2.2. Write a program that provides the direction vector of a ball seen by the camera (in the camera reference frame). 

1- Fork and clone the following repository: https://github.com/esdalar/ros_img_processor.git
2- Change the requiered values of our camera in the file "*launch/usb_camera.launch*". In this case, we should change only the value of the used camera "*device0*":

<launch>
	<!-- Launch usb camera-->
	<!-- See parameter definition at http://wiki.ros.org/usb_cam -->
    <node name="usb_cam" pkg="usb_cam" type="usb_cam_node" output="screen" >
        <param name="video_device" value="/dev/video0" />
        <param name="image_width" value="640" />
        <param name="image_height" value="480" />
        <param name="pixel_format" value="yuyv" />
        <param name="camera_frame_id" value="usb_cam" />
        <param name="io_method" value="mmap"/>
    </node>

	<!-- image viewer -->
    <node
        name="image_view_in"
        pkg="image_view"
        type="image_view"
        respawn="false"
        output="screen">
        <remap from="image" to="/usb_cam/image_raw"/>
        <param name="autosize" value="false" />
    </node>

</launch>

3- After changing this value run the program:

```sh
$ roslaunch ros_img_processor ros_img_processor.launch
```


