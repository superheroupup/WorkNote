###October 11, 2018 3:05 PM

#### 1.rviz中无法正确的选择kinect的坐标系
```
roslaunch kinect2_bridge kinect2_bridge.launch publish_tf:=true
```
**启动的时候发布tf**

###October 16, 2018 10:11 AM
####手眼标定
ArUco marker生成器
1.启动ur机器人时要把参数llimited去掉
2.将所有的节点写在一个启动文件时，采图会导致节点挂掉，将节点分开后可正常运行。。。。（坑）
  ```
  roslaunch kinect2_bridge kinect2_bridge.launch
  roslaunch easy_handeye startUr.launch
  roslaunch easy_handeye startArUco.launch
  roslaunch easy_handeye my.launch
  rqt_image_view  选择话题： /aruco/track/result
  ```

### ros点云 octomap
1.在easy_handeye/launch中添加mypublish.launch
```
<launch>
  <!-- start the robot -->
  <include file="$(find ur_modern_driver)/launch/ur5_bringup.launch">
      <arg name="robot_ip" value="192.168.0.5" />
  </include>

  <include file="$(find ur5_moveit_config)/launch/ur5_moveit_planning_execution.launch">
  </include>
 
  <include file="$(find ur5_moveit_config)/launch/moveit_rviz.launch">
      <arg name="config" value="true" />
  </include>
 
  <include file="$(find kinect2_bridge)/launch/kinect2_bridge.launch" />
 
  <node pkg="tf" type="static_transform_publisher" name="ur5_broadcaster" args="-1.1101661573873884 0.4983191170305918 -0.15058763625256227 -0.2065729507929449 0.6210725240958181 -0.7083737516551495 0.2642028837464752 base_link kinect2_rgb_optical_frame 100" />

</launch>

```
**static_transform_publisher发布坐标系转换，args的值在上面手眼标定后按save会在~/.ros/easy_handeye路径下保存一个文件，将该文件中的值替换到此处，顺序：x,y,x,qx,qy,qz,qw**

2.在ur5_moveit_config/config中添加sensors_kinect.yaml
```
sensors:
  - sensor_plugin: occupancy_map_monitor/PointCloudOctomapUpdater
    point_cloud_topic: /kinect2/qhd/points
    max_range: 5.0
    point_subsample: 1
    padding_offset: 0.1
    padding_scale: 1.0
    max_update_rate: 1.0
    filtered_cloud_topic: filtered_cloud
```
只需修改point_cloud_topic为对应的点云主题

3.修改ur5_moveit_config/launch中的sensor_manager.launch.xml，在文件末尾添加如下代码
```
  <rosparam command="load" file="$(find ur5_moveit_config)/config/sensors_kinect.yaml" />
  <param name="octomap_frame" type="string" value="odom_combined" />
  <param name="octomap_resolution" type="double" value="0.05" />
  <param name="max_range" type="double" value="5.0" />
```
4.启动
```
  roslaunch easy_handeye mypublish.launch
```

### October 17, 2018 6:08 PM
#### ros kinect2 ork linemod
```
roslaunch kinect2_bridge kinect2_bridge.launch
rosrun topic_tools relay /kinect2/qhd/image_depth_rect /camera/depth_registered/image_raw
rosrun topic_tools relay /kinect2/qhd/image_color_rect /camera/rgb/image_rect_color
rosrun topic_tools relay /kinect2/qhd/camera_info /camera/rgb/camera_info
rosrun topic_tools relay /kinect2/qhd/camera_info /camera/depth_registered/camera_info
rosrun topic_tools relay /kinect2/qhd/points /camera/depth_registered/points
rosrun tf static_transform_publisher 0 0 0 0 0 0 kinect2_ir_opticalrame camera_depth_optical_frame 40
--rosrun object_recognition_core detection -c  `rospack find object_recognition_linemod`/conf/detection.ros.ork (成功)
--rosrun object_recognition_core detection -c  `rospack find object_recognition_tabletop`/conf/detection.object.ros.ork (未成功
```
