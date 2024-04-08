# ROS1 imu function package (WHEELTWC n100)

### 1.Add dependencies：
#### 1.1 serial module installation
[ros-foxy,ros-noetic Version of serial module installation](https://icode.best/i/32316244547594)

ros-melodic version:
```bash
sudo apt install ros-melodic-serial
```
#### 1.2 CH343 Driver Installation

[linux下CH343 Module driver installation](https://github.com/WCHSoftGroup/ch343ser_linux)

**假如你会发生以下ERROR**

>insmod: ERROR: could not insert module ch343.ko: Operation not permitted

+ 需要进入bios界面 然后将安全模式关掉 否则无法insmod

>假如内核版本过高编译失败(compilation error on pop os 21.10 with kernal 5.16.15)

+ [高版本驱动](https://github.com/GreatestCapacity/ch343ser_linux) 这个编译有问题 代码稍微改点就可以编译了
### 2.修改CH9102 固定串口号

[修改为固定名称](./wheeltec_udev.sh)

```shell
sudo sh wheeltec_udev.sh
```

### 3.使用：    
##### 3.1 编译
```shell
catkin_make -DCATKIN_WHITELIST_PACKAGES=fdilink_ahrs # 或者使用catkin build
```
##### 3.2 启动
```shell
roslaunch fdilink_ahrs  ahrs_driver.launch
```
[查看ahrs_driver.launch配置](./launch/ahrs_driver.launch)

**其中`device_type`：**

+ 0. Deta-10的原始坐标系模式
+ 1. 单独imu的坐标系模式


##### 3.3 查看数据
```shell
rostopic echo /imu
```


**调用的ahrs_driver节点会发布`sensor_msgs/Imu`格式的imu** topic。
```
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Quaternion orientation
  float64 x
  float64 y
  float64 z
  float64 w
float64[9] orientation_covariance
geometry_msgs/Vector3 angular_velocity
  float64 x
  float64 y
  float64 z
float64[9] angular_velocity_covariance
geometry_msgs/Vector3 linear_acceleration
  float64 x
  float64 y
  float64 z
float64[9] linear_acceleration_covariance
```
也会发布`geometry_msgs/Pose2D`格式的二维指北角话题，话题名默认为`/mag_pose_2d`。
```
float64 x
float64 y
float64 theta  # 指北角
```
### 4. Quick guid to run the driver for n100 IMU
##### 4.1 Install ROS
You can find the full instalition steps in ROS wiki http://wiki.ros.org/melodic/Installation/Ubuntu
##### 4.2 Create ROS workspace 
(change it to your ROS1 distrbution, I use melodic dist.)
source /opt/ros/melodic/setup.bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
##### 4.3 Add the driver to the workspace
sudo apt install ros-melodic-serial
cd ~/catkin_ws/src
git clone 
cd ~/catkin_ws/src/fdilink_ahrs
sudo sh wheeltec_udev.sh
cd ../../
catkin_make -DCATKIN_WHITELIST_PACKAGES=fdilink_ahrs
##### 4.4 Launch the driver
roscore
roslaunch fdilink_ahrs  ahrs_driver.launch



