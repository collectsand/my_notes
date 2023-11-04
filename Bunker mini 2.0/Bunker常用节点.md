***内容由 ai 生成，正确性存疑*** #不确定内容

1. cmd_vel

接收速度指令的话题,geometry_msgs/Twist类型。

2. odom

发布机器人里程计数据的话题,nav_msgs/Odometry类型。

3. scan

雷达扫描数据,sensor_msgs/LaserScan类型。

4. map

地图数据,nav_msgs/OccupancyGrid类型。

5. amcl_pose

定位信息,geometry_msgs/PoseWithCovarianceStamped类型。

6. /bunker_status

bunker状态数据,bunker_msgs/BunkerStatus类型。

7. /tf

坐标变换,tf/tfMessage类型。

8. /joint_states

关节状态,sensor_msgs/JointState类型。

9. /camera/image_raw

摄像头图像,sensor_msgs/Image类型。

这些是 bunker 在导航、控制、定位、感知等过程中常用的 ROS 标准话题类型。可以用于接入 bunker 的传感器信息,以及发送控制指令。

