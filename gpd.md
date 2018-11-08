### October 17, 2018 11:58 AM
1.编译时报错 fatal error: caffe/proto/caffe.pb.h: No such file or directory
```
解决：
# In the directory you installed Caffe to
protoc src/caffe/proto/caffe.proto --cpp_out=.
mkdir include/caffe/proto
mv src/caffe/proto/caffe.pb.h include/caffe/proto
```

2.启动
```
roslaunch kinect2_bridge kinect2_bridge.launch publish_tf:=true
roslaunch kinect2_bridge kinect2_modifytopic.launch
roslaunch gpd tutorial1.launch
rviz  然后open config  : /home/efun/work/gpd_test/src/gpd/tutorials/openni2.rviz
```
3.启动检测一张点与图
```
roslaunch gpd tutorial2.launch
python src/gpd/scripts/select_grasp.py
rosrun pcl_ros pcd_to_pointcloud src/gpd/tutorials/krylon.pcd
rosrun pcl_ros pcd_to_pointcloud src/gpd/tutorials/trace-obs.pcd 
rosrun pcl_ros pcd_to_pointcloud src/gpd/tutorials/trace-type.pcd 
```

4.实时抓取实验
```
roslaunch kinect2_bridge kinect2_bridge.launch publish_tf:=true
roslaunch kinect2_bridge kinect2_modifytopic.launch
roslaunch gpd tutorial1.launch
rviz  然后open config  : /home/efun/work/gpd_test/src/gpd/tutorials/openni2.rviz
roslaunch easy_handeye mypublish.launch
roslaunch ur_controller ur_real_start.launch
rosrun  ur_controller ur_demo
```
