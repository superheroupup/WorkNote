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
