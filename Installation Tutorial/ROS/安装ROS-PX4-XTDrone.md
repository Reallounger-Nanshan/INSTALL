# 安装 ROS-PX4-XTDrone

## (〇) 环境

* Ubuntu18.04

* ROS-melodic

## (一) 安装依赖

### 1.1 系统环境相关依赖

* ```
  sudo apt install ninja-build exiftool ninja-build protobuf-compiler libeigen3-dev genromfs xmlstarlet libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev python-pip python3-pip gawk

### 1.2 Python 相关依赖

* ```
  pip2 install pandas jinja2 pyserial cerberus pyulog==0.7.0 numpy toml pyquaternion empy pyyaml 

* ```
  pip3 install packaging numpy empy toml pyyaml jinja2 pyargparse

### 1.3 源码编译 CMake

#### 1.3.0 注意事项

* PX4 要求 CMake 版本再 3.10-3.16 之间
* 多版本 CMake 建议均使用源码编译——通过 sudo make install 切换版本
  * 如果是和系统环境的 CMake 进行版本切换则需要修改软链接

#### 1.3.1 下载

* 下载地址：[Index of /files (cmake.org)](https://cmake.org/files/)
* 下载完成后解压到目标路径

#### 1.3.2 编译

* ```
  ./bootstrap
  ```

* ```
  make -j16

* ```
  sudo make install
  ```

#### 1.3.3 查看当前 CMake 版本

* ```
  cmake --version



## (二) 下载 XTDrone 源码

### 2.1 下载

* ```
  git clone https://gitee.com/robin_shaun/XTDrone.git

### 2.2 安装子模块

* ```
  cd XTDrone
  ```

* ```
  git submodule update --init --recursive



## (三) 安装 Gazebo

### 3.1 安装 Gazebo9

#### 3.1.1 卸载先前的 Gazebo

* ```
  sudo apt-get remove gazebo* 

* ```
  sudo apt-get remove libgazebo*
  ```

* ```
  sudo apt-get remove ros-melodic-gazebo*

#### 3.1.2 安装

##### a. 导入仓库

* ```
  sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
  ```

* ```
  deb http://packages.osrfoundation.org/gazebo/ubuntu-stable bionic main

##### b. 设置 keys

* ```
  wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

##### c. 安装

* ```
  sudo apt-get update
  ```

* ```
  sudo apt-get install gazebo9
  ```

* ```
  sudo apt-get install libgazebo9-dev
  ```

##### d. 查看是否安装成功

* ```
  gazebo

### 3.2 安装 ROS 插件

#### 3.2.1 安装依赖

* ```
  sudo apt-get install ros-melodic-moveit-msgs ros-melodic-object-recognition-msgs ros-melodic-octomap-msgs ros-melodic-camera-info-manager  ros-melodic-control-toolbox ros-melodic-polled-camera ros-melodic-controller-manager ros-melodic-transmission-interface ros-melodic-joint-limits-interface

#### 3.2.2 编译

* ```
  cd ~/catkin_ws

* ```
  cp -r ~/XTDrone/sitl_config/gazebo_ros_pkgs src/
  ```

* ```
  catkin build

#### 3.2.3 环境配置

* ```
  gedit ~/.bashrc

* ```bash
  # 添加以下命令
  
  source ~/catkin_ws/devel/setup.bash
  ```

* ```
  source ~/.bashrc



## (四) 安装 MAVROS

### 4.1 安装 MAVROS

* ```
  sudo apt install ros-melodic-mavros ros-melodic-mavros-extras

### 4.2 安装 Geographiclib

#### 4.2.0 备注

* 这一步也可以直接将文件放到目标位置从而跳过

#### 4.2.1 下载

* ```
  wget https://gitee.com/robin_shaun/XTDrone/raw/master/sitl_config/mavros/install_geographiclib_datasets.sh

#### 4.2.2 修改权限

* ```
  sudo chmod a+x ./install_geographiclib_datasets.sh

#### 4.2.3 安装

* ```
  sudo ./install_geographiclib_datasets.sh



## (五) 编译 PX4 源码

### 5.1 下载

#### 5.1.1 下载源码

* ```
  git clone https://github.com/PX4/PX4-Autopilot.git
  ```

* ```
  mv PX4-Autopilot PX4_Firmware

#### 5.1.2 下载子模块

* ```
  cd PX4_Firmware
  ```

* ```
  git checkout -b xtdrone/dev v1.11.0-beta1
  ```

* ```
  git submodule update --init --recursive

#### 5.1.3 编译 PX4 固件

* ```
  make px4_sitl_default gazebo

#### 5.2 环境配置

* ```
  gedit ~/.bashrc

* ```bash
  # 添加以下命令
  
  source ~/catkin_ws/devel/setup.bash
  source ~/PX4_Firmware/Tools/setup_gazebo.bash ~/PX4_Firmware/ ~/PX4_Firmware/build/px4_sitl_default
  export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4_Firmware
  export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4_Firmware/Tools/sitl_gazebo
  ```

* ```
  source ~/.bashrc

#### 5.3 验证安装成功

* ```
  roslaunch px4 mavros_posix_sitl.launch



## (六) XTDrone 配置

### 6.1 拷贝文件

* ```
  cp sensing/gimbal/gazebo_gimbal_controller_plugin.cpp ~/PX4_Firmware/Tools/sitl_gazebo/src/
  ```

* ```
  cp sitl_config/init.d-posix/rcS ~/PX4_Firmware/ROMFS/px4fmu_common/init.d-posix/
  ```

* ```
  cp sitl_config/worlds/* ~/PX4_Firmware/Tools/sitl_gazebo/worlds/
  ```

* ```
  cp -r sitl_config/models/* ~/PX4_Firmware/Tools/sitl_gazebo/models/ 
  ```

* ```
  cp -r sitl_config/launch/* ~/PX4_Firmware/launch/

### 6.2 删除文件

* ```
  cd ~/.gazebo/models/
  ```

* ```
  rm -r stereo_camera/ 3d_lidar/ 3d_gpu_lidar/ hokuyo_lidar/

### 6.3 编译 PX4 固件

#### 6.3.0 说明

* 由于修改了 PX4 sitl_gazebo 中的 gazebo_gimbal_controller_plugin.cpp（源代码不能控制多无人机的云台），需要再编译一次

#### 6.3.1 编译

* ```
  cd ~/PX4_Firmware

* ```
  make px4_sitl_default gazebo



## (七) 安装 QGC (QGroundControl) 地面站

### 7.1 安装依赖

* ```
  sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav -y

### 7.2 下载 QGC

* https://s3-us-west-2.amazonaws.com/qgroundcontrol/latest/QGroundControl.AppImage

### 7.3 修改权限

* ```
  chmod +x ./QGroundControl.AppImage



## (八) Gazebo 仿真

### 8.1 安装 Gazebo 插件

* ```
  sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev

### 8.2 运行仿真

* ```
  cd ~/PX4-Autopilot

* ```
  make px4_sitl_default gazebo

### 8.3 创建外部控制程序包

#### 8.3.1 创建 offboard 包

* ```
  cd ~/ros_ws/src
  ```

* ```
  catkin_create_pkg offboard roscpp mavros geometry_msgs

#### 8.3.2 创建 offboard_node.cpp

* 在offboard/src目录下创建offboard_node.cpp

  * ```c++
    /**
     * @file offb_node.cpp
     * @brief Offboard control example node, written with MAVROS version 0.19.x, PX4 Pro Flight
     * Stack and tested in Gazebo SITL
     */
    
    #include <ros/ros.h>
    #include <geometry_msgs/PoseStamped.h>
    #include <mavros_msgs/CommandBool.h>
    #include <mavros_msgs/SetMode.h>
    #include <mavros_msgs/State.h>
    
    mavros_msgs::State current_state;
    void state_cb(const mavros_msgs::State::ConstPtr& msg){
        current_state = *msg;
    }
    
    int main(int argc, char **argv)
    {
        ros::init(argc, argv, "offb_node");
        ros::NodeHandle nh;
    
        ros::Subscriber state_sub = nh.subscribe<mavros_msgs::State>
                ("mavros/state", 10, state_cb);
        ros::Publisher local_pos_pub = nh.advertise<geometry_msgs::PoseStamped>
                ("mavros/setpoint_position/local", 10);
        ros::ServiceClient arming_client = nh.serviceClient<mavros_msgs::CommandBool>
                ("mavros/cmd/arming");
        ros::ServiceClient set_mode_client = nh.serviceClient<mavros_msgs::SetMode>
                ("mavros/set_mode");
    
        //the setpoint publishing rate MUST be faster than 2Hz
        ros::Rate rate(20.0);
    
        // wait for FCU connection
        while(ros::ok() && !current_state.connected){
            ros::spinOnce();
            rate.sleep();
        }
    
        geometry_msgs::PoseStamped pose;
        pose.pose.position.x = 0;
        pose.pose.position.y = 0;
        pose.pose.position.z = 2;
    
        //send a few setpoints before starting
        for(int i = 100; ros::ok() && i > 0; --i){
            local_pos_pub.publish(pose);
            ros::spinOnce();
            rate.sleep();
        }
    
        mavros_msgs::SetMode offb_set_mode;
        offb_set_mode.request.custom_mode = "OFFBOARD";
    
        mavros_msgs::CommandBool arm_cmd;
        arm_cmd.request.value = true;
    
        ros::Time last_request = ros::Time::now();
    
        while(ros::ok()){
            if( current_state.mode != "OFFBOARD" &&
                (ros::Time::now() - last_request > ros::Duration(5.0))){
                if( set_mode_client.call(offb_set_mode) &&
                    offb_set_mode.response.mode_sent){
                    ROS_INFO("Offboard enabled");
                }
                last_request = ros::Time::now();
            } else {
                if( !current_state.armed &&
                    (ros::Time::now() - last_request > ros::Duration(5.0))){
                    if( arming_client.call(arm_cmd) &&
                        arm_cmd.response.success){
                        ROS_INFO("Vehicle armed");
                    }
                    last_request = ros::Time::now();
                }
            }
    
            local_pos_pub.publish(pose);
    
            ros::spinOnce();
            rate.sleep();
        }
    
        return 0;
    }

#### 8.3.3 修改 CMakeLists.txt 文件

* ```cmake
  add_executable(offboard_node src/offboard_node.cpp)
  ```

* ```cmake
  target_link_libraries(offboard_node ${catkin_LIBRARIES})

#### 8.3.4 编译

* ```
  cd ~/ros_ws
  ```

* ```
  catkin_make

### 8.4 启动仿真

#### 8.4.1 切换到固件目录

* ```
  cd ~/PX4-Autopilot
  ```

#### 8.4.2 启动 Gazebo 仿真

* ```
  make px4_sitl_default gazebo
  ```

#### 8.4.3 启动 MAVROS 链接到本地 ROS

* ```
  roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"
  ```

#### 8.4.4 运行外部控制程序

* ```
  rosrun offboard offboard_node
  ```

* 成功运行后能在 Gazebe 仿真里看到无人机飞到高度 2m 处