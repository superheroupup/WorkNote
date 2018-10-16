### October 11, 2018 3:05 PM

#### rviz中无法正确的选择kinect的坐标系
```
roslaunch kinect2_bridge kinect2_bridge.launch publish_tf:=true
```
**启动的时候发布tf**

### October 16, 2018 10:11 AM
#### 手眼标定
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
