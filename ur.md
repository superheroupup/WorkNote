### 修改
### September 13, 2018 10:10 AM
修改了 ur5_robotiq_parallel_moveit/config/controllers.yaml 为
```
	controller_list:
  # - name: arm_controller
  - name: ""
    action_ns: follow_joint_trajectory
    type: FollowJointTrajectory
    joints:
      - shoulder_pan_joint
      - shoulder_lift_joint
      - elbow_joint
      - wrist_1_joint
      - wrist_2_joint
      - wrist_3_joint

#  - name: gripper_action
#     type: GripperCommand
#     action_ns: gripper_action
#     default: true
#     joints:
#       - robotiq_85_left_knuckle_joint
```
现在可以实际操控机器人，担任然有很多 warn ，待解决。。。

### September 20, 2018 4:11 PM
- 修改 ur5_robotiq_parallel_joint_limited.xacro 和 ur5_robotiq_parallel.xacro
```
  将origin z 由0修改为-0.375，让机械臂原点在网格上
<joint name="world_to_stand" type="fixed">
		<parent link="world"/>
		<child link="stand"/>
		<origin xyz="0 0 -0.375" rpy="0 0 0"/>
	</joint>
```
```
	修改 origin rpy 使得模拟与现实机器人处于相同方向，具体还需研究
	<joint name="stand_to_base" type="fixed">
		<parent link="stand"/>
		<!--child link="${arm_prefix}base_link"/-->
		<child link="base_link"/>
		<origin xyz="0 0 ${robot_stand_height * 0.5}" rpy="0 0 0"/>
		<!--<origin xyz="0 0 ${robot_stand_height * 0.5}" rpy="0 0 1.57"/>-->
		<!--<origin xyz="0 0 0" rpy="0 0 1.57"/>-->
	</joint>
```

目前情况：
1. 很多warn还是未解决
2. 运动不平滑，易出现保护性停止，怀疑与kinematics.yaml配置有光
3. 视觉识别后给出的点，运动不正确，怀疑识别出错

### September 20, 2018 6:26 PM
- 修改 ur5_robotiq_parallel_joint_limited.xacro 和 ur5_robotiq_parallel.xacro
```
  修改 origin rpy为3.14后与真实机器人方向一致，具体原因不知道。。。。。。
	<joint name="stand_to_base" type="fixed">
		<parent link="stand"/>
		<!--child link="${arm_prefix}base_link"/-->
		<child link="base_link"/>
		<origin xyz="0 0 ${robot_stand_height * 0.5}" rpy="0 0 3.14"/>
		<!--<origin xyz="0 0 ${robot_stand_height * 0.5}" rpy="0 0 1.57"/>-->
		<!--<origin xyz="0 0 0" rpy="0 0 1.57"/>-->
	</joint>
```
