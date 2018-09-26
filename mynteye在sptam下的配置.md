### mynteye下使用sptam
#### install sptam
```
1.sptam仓库地址:https://github.com/lrse/sptam
2.每个附属包都需要像g2o包那样进行build
```
#### install mynteye sdk
```
1.sdk(2.0)仓库地址：https://github.com/slightech/MYNT-EYE-SDK
  skd(1.8)仓库地址：https://www.cnblogs.com/kekeoutlook/p/8619896.html
2.mynteye标定: 相机baseline = 12cm (sptam下显示应该为0.12即证明标定准确)
  2.1- 用ros标定(标定结果未成功写入相机):http://wiki.ros.org/camera_calibration/Tutorials/StereoCalibration
  2.2- 用sdk1.8进行标定(s=0.002):https://slightech.github.io/MYNT-EYE-SDK/calibrate_with_opencv.html,若用sdk2.0,则需使用[这个网址](http://doc.myntai.com/resource/sdk/mynt-eye-sdk-guide-2.0.1-rc0-html-zh-Hans/mynt-eye-sdk-guide-2.0.1-rc0-html-zh-Hans/src/data/write_img_params.html)将标定结果写入相机
```

#### run mynteye
```
2.0版本
roslaunch mynt_eye_ros_wrapper mynteye.launch
```

#### run sptam
```
1.若实时建图，则将sptam包下的kitti.launch文件中的 use_sim_time 改为false. 若运行rosbag包，则修改为true.
2.将sptam包下的kitti.launch 文件中的图像话题修改
    <remap from="/stereo/left/image_rect" to="/mynteye/left/image_rect" />
    <remap from="/stereo/right/image_rect" to="/mynteye/right/image_rect" />
    <remap from="/stereo/left/camera_info"  to="/mynteye/left/camera_info" />
    <remap from="/stereo/right/camera_info"  to="/mynteye/right/camera_info" />
3.  修改sptam包中的configurationFiles/kitti.yaml文件,将EpipolarDistance的值修改为4.0
4.roslaunch sptam kitti.launch
5.rviz 添加 观看话题 path 和 pointcloud
  5.1 Fixed Frame as map    
  5.2 Add -> By Topic -> PointCloud
  5.3 Add -> By Topic -> Path   
```
 **注 ：如果S-PTAM仍然丢失，请尝试更改一些配置参数。例如，可以将稍微增加kitti.yaml文件中的MatchingNeighborhood和/或MatchingDistance。还可尝试增加FrustumFarPlaneDist。最后，可以尝试使关键帧创建策略更加灵活，从而减少minimumTrackedPointsRatio值**
