## 安装 ROS-PX4

### (〇) 环境

* Ubuntu18.04

* ROS-melodic



### (一) 安装 MAVROS

#### 1.  安装编译工具

* ```
  sudo apt-get install python-catkin-tools python-rosinstall-generator -y

#### 2. 创建工作空间

* 建议单独创建一个工作空间——安装完后该工作空间只能用catkin build而不能用catkin_make

* ```
  mkdir -p ~/mavros_ws/src && cd ~/mavros_ws

* ```
  catkin init

* ```
  wstool init src

#### 3. 更新

* 更新保证获取到最新的稳定版mavlink和mavros

* ```
  rosinstall_generator --rosdistro melodic mavlink | tee /tmp/mavros.rosinstall

* ```
  rosinstall_generator --upstream mavros | tee -a /tmp/mavros.rosinstall

#### 4. 归并工作链，准备开始安装

* ```
  wstool merge -t src /tmp/mavros.rosinstall

* ```
  wstool update -t src -j20

* ```
  rosdep install --from-paths src --ignore-src -y

#### 5. 安装 GeographicLib datasets

* ```
  sudo ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh

* 或者把文件夹直接拷贝到 /usr/share

#### 6. 编译源码

* ```
  catkin build

#### 7. 配置环境

* ```
  sudo gedit ~/.bashrc

* ```bash
  # 在安装ROS创建工作空间时添加的语句下添加
  source ~/mavros_ws/devel/setup.bash

* ```
  source ~/.bashrc



### (二) 安装 FastRTPS 和 Nuttx

#### 1. 权限设置

* ```
  sudo usermod -a -G dialout $USER

* 注销，重新登录

#### 2. 安装必备软件

* ```
  sudo apt-get update

* ```
  sudo apt-get install python-argparse git wget zip python-empy qtcreator cmake build-essential genromfs -y

#### 3. 安装仿真工具

* ```
  sudo add-apt-repository ppa:openjdk-r/ppa
  ```

* ```
  sudo apt-get update

* ```
  sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang-3.9 liblldb-3.9 -y

#### 4. 卸载模式管理器

* ```
  sudo apt-get remove modemmanager

#### 5. 安装依赖包

* ```
  sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa

* ```
  sudo apt-get update

* ```
  sudo apt-get install python-serial openocd flex bison libncurses5-dev autoconf texinfo libftdi-dev libtool zlib1g-dev gcc-arm-none-eabi -y

#### 6. 安装 python 包

* ```
  sudo apt-get update -y

* ```
  sudo apt-get install python-toml python-numpy python-dev python-pip -y

* ```
  sudo -H pip install --upgrade pip

* ```
  sudo -H pip install pandas jinja2 pyserial

* ```
  sudo apt install python3-pip

* ```
  sudo pip3 install numpy toml
  ```

* ```
  pip3 install --user empy

* ```
  sudo pip3 install jinja2

* ```
  sudo -H pip install pyulog

#### 7. 安装Ninja编译工具

* ```
  sudo apt-get install ninja-build -y

#### 8. 安装FastRTPS开发环境

* 下载：[eProsima Fast DDS 1.7.1](https://www.eprosima.com/index.php/component/ars/repository/eprosima-fast-dds/eprosima-fast-rtps-1-7-1)

* ```
  tar -xzf eProsima_FastRTPS-1.7.1-Linux.tar.gz eProsima_FastRTPS-1.7.1-Linux/

* ```
  tar -xzf eProsima_FastRTPS-1.7.1-Linux.tar.gz requiredcomponents

* ```
  tar -xzf requiredcomponents/eProsima_FastCDR-1.0.8-Linux.tar.gz

* ```
  cd eProsima_FastCDR-1.0.8-Linux

* ```
  ./configure --libdir=/usr/lib

* ```
  make -j20

* ```
  sudo make install

* ```
  cd ../eProsima_FastRTPS-1.7.1-Linux

* ```
  ./configure --libdir=/usr/lib

* ```
  make -j20

* ```
  sudo make install

* ```
  cd ..

* ```
  rm -rf requiredcomponents eprosima_fastrtps-1-7-1-linux.tar.gz

#### 9. 配置 arm gcc 编译器

* ```
  sudo apt-get remove gcc-arm-none-eabi gdb-arm-none-eabi binutils-arm-none-eabi gcc-arm-embedded

* 如果找不到gdb

  * ```
    sudo apt install gdb-multiarch

* ```
  sudo add-apt-repository --remove ppa:team-gcc-arm-embedded/ppa

* ```bash
  新建脚本 script.sh 并粘贴
  
  pushd .cd ~
  wget https://armkeil.blob.core.windows.net/developer/Files/downloads/gnu-rm/7-2017q4/gcc-arm-none-eabi-7-2017-q4-major-linux.tar.bz2
  tar -jxf gcc-arm-none-eabi-7-2017-q4-major-linux.tar.bz2
  exportline="export PATH=$HOME/gcc-arm-none-eabi-7-2017-q4-major/bin:\$PATH"
  if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
  popd
  
  bash script.sh

* ```
  reboot

#### 10. 检查是否安装成功

* ```
  arm-none-eabi-gcc --version

* 如果输出如下例所示则成功

  * ```shell
    arm-none-eabi-gcc (GNU Tools for Arm Embedded Processors 7-2017-q4-major) 7.2.1 20170904 (release) [ARM/embedded-7-branch revision 255204]
    Copyright (C) 2017 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions. There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.



### (三) 编译 PX4 源码

#### 1. 下载 PX4 源码

* 选择合适的 PX4 版本：[GitHub - PX4/PX4-Autopilot: PX4 Autopilot Software](https://github.com/PX4/PX4-Autopilot)

* ```
  git clone -b v1.11.3 https://github.com/PX4/PX4-Autopilot.git

* ```
  cd PX4-Autopilot

* ```
  git submodule update --init --recursive

#### 2. 下载 XTDrone 源码

* ```
  cd ~

* ```
  git clone https://gitee.com/robin_shaun/XTDrone.git
  ```

* ```
  cd XTDrone

* ```
  git submodule update --init --recursive

#### 3. 拷贝文件

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

* ```
  cp -r sitl_config/launch/* ~/PX4_Firmware/launch/

#### 4. 删除文件

* 没有文件可删除，不影响使用

* ```
  cd ~/.gazebo/models/
  ```

* ```
  rm -r stereo_camera/ 3d_lidar/ 3d_gpu_lidar/ hokuyo_lidar/

#### 5. 编译常用固件

* ```
  cd ~/PX4_Firmware
  ```

* ```
  make clean
  ```

  * 未编译过固件跳过这一步

* ```
  make px4_fmu-v5_default

* 如果出现报错：'i' does not name a type   __ULong i[2];

  * ```python
    sudo gedit /usr/include/newlib/math.h
    
    # 添加（定义__ULong的数据类型）
    #define  __ULong long
    ```

* 如果出现报错：parameter ‘expected’ set but not used

  * ```python
    sudo gedit PX4-Autopilot\cmake\px4_add_common_flags.cmake
    
    # 注释-Werror行

  * 会提示Warning，但可编译通过

  * 该方法仅避免将该问题做error处理，事实上并没有解决问题



### (四) 安装 QGC (QGroundControl) 地面站

#### 1. 安装依赖

* ```
  sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav -y

#### 2. 下载 QGC

* https://s3-us-west-2.amazonaws.com/qgroundcontrol/latest/QGroundControl.AppImage

#### 3. 安装（赋予权限）

* ```
  chmod +x ./QGroundControl.AppImage



### (五) Gazebo 仿真

#### 1. Gazebo 配置

* ```
  sudo apt-get install libprotobuf-dev libprotoc-dev

* ```
  cd ~/PX4-Autopilot/Tools/sitl_gazebo

* ```
  mkdir build

#### 2. 添加路径

* ```
  sudo gedit ~/.bashrc

* ```bash
  # 添加
  
  source ~/PX4-Autopilot/Tools/setup_gazebo.bash ~/PX4-Autopilot ~/PX4-Autopilot/build/px4_sitl_default
  source /usr/share/gazebo-9/setup.sh
  
  export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot:/home/wzy/src/PX4-Autopilot/Tools/sitl_gazebo
  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/.gazebo/models
  export SITL_GAZEBO_PATH=$HOME/PX4-Autopilot/Tools/sitl_gazebo
  export GAZEBO_RESOURCE_PATH=${GAZEBO_RESOURCE_PATH}:$HOME/PX4-Autopilot/Tools/sitl_gazebo/worlds
  export GAZEBO_MODEL_DATABASE_URI=""
  export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:$HOME/PX4-Autopilot/Tools/sitl_gazebo/build
  ```

  * ```bash
    # 设置插件的路径以便 Gazebo 能找到模型文件
    export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:$HOME/PX4-Autopilot/Tools/sitl_gazebo/build
    
    # 设置模型路径以便 Gazebo 能找到机型
    export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:$HOME/PX4-Autopilot/Tools/sitl_gazebo/models
    
    # 禁用在线模型查找
    export GAZEBO_MODEL_DATABASE_URI=""
    
    # 设置 sitl_gazebo 文件夹的路径
    export SITL_GAZEBO_PATH=$HOME/PX4-Autopilot/Tools/sitl_gazebo

* ```
  source ~/.bashrc
  ```

  * ```shell
    # 终端会显示
    
    GAZEBO_PLUGIN_PATH :~/PX4-Autopilot/build/px4_sitl_default/build_gazebo 
    GAZEBO_MODEL_PATH :~/PX4-Autopilot/Tools/sitl_gazebo/models
    LD_LIBRARY_PATH :~/mavros_ws/devel/lib:/opt/ros/melodic/lib:~/PX4-Autopilot/build/px4_sitl_default/build_gazebo

  * ```bash
    gedit ~/PX4-Autopilot/Tools/setup_gazebo.bash
    
    # 注释echo语句

#### 3. CMake

* ```
  cd build

* ```
  cmake ..

#### 4. 安装 Gazebo 插件

* ```
  sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev

#### 5. 运行仿真

* ```
  cd ~/PX4-Autopilot

* ```
  make px4_sitl_default gazebo

#### 6. 创建外部控制程序包

* ```
  cd ~/ros_ws/src

* ```
  catkin_create_pkg offboard roscpp mavros geometry_msgs

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

* 修改CMakeLists.txt文件

  * ```cmake
    add_executable(offboard_node src/offboard_node.cpp)
    ```

  * ```cmake
    target_link_libraries(offboard_node ${catkin_LIBRARIES})

* ```
  cd ~/ros_ws

* ```
  catkin_make

#### 7. 启动仿真

* 切换到固件目录

  * ```
    cd ~/PX4-Autopilot

* 启动gazebo仿真

  * ```
    make px4_sitl_default gazebo

* 启动MAVROS,链接到本地ROS

  * ```
    roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"

* 运行外部控制程序

  * ```
    rosrun offboard offboard_node

* 成功运行后能在Gazebo仿真里看到无人机飞到高度2m处