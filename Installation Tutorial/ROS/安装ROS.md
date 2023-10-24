# 安装 ROS

## (一) 鱼香 ROS 一键安装（推荐）

* ```
  wget http://fishros.com/install -O fishros && . fishros
  ```

  * 支持范围
    * 一键安装: ROS (支持 ROS 和 ROS2，树莓派 Jetson)
    * 一键安装: VsCode (支持 amd64 和 arm64)
    * 一键安装: github 桌面版
    * 一键安装: nodejs 开发环境
    * 一键配置: rosdep (小鱼的 rosdepc，又快又好用)
    * 一键配置: ROS环境 (快速更新 ROS 环境设置,自动生成环境选择)
    * 一键配置: 系统源 (更换系统源,支持全版本 Ubuntu 系统)
    * 一键安装: Docker (支持 amd64 和 arm64)
    * 一键安装: cartographer
    * 一键安装: 微信客户端



## (二) 官方步骤

### 1. 安装

#### 1.2 添加 sources.list

* ```
  sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
  ```

#### 1.3 添加 keys

* ```
  sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
  ```

#### 1.4 系统更新

* ```
  sudo apt-get update
  ```

#### 1.5 安装 ROS

* ```
  sudo apt-get install ros-melodic-desktop-full
  ```

### 2. 配置

#### 2.1 初始化 rosdep

* ```
  sudo rosdep init
  ```

* 如果出现报错：sudo rosdep：找不到命令提示

  * ```
    sudo apt-get install python-rosdep python-wstool ros-melodic-ros

* 如果出现报错：cannot download default sources list from:  https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list

  * ```
    sudo gedit /etc/hosts
    ```

  * ```
    # 在文件末尾添加
    151.101.84.133 raw.githubusercontent.com

* ```
  rosdep update

* 如果出现报错：urlopen error timed out
  * 更改网络

* 若实在无法更新可使用：(用国内链接实现初始化)

  * ```
    sudo pip install rosdepc
    ```

  * ```
    sudo rosdepc init
    ```

  * ```
    rosdepc update

#### 2.2 ROS 环境配置

* ```
  echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
  ```

* ```
  source ~/.bashrc

#### 2.3 安装 rosinstall

* ```
  sudo apt-get install python-rosinstall
  ```
  
* 如果出现报错：

  * ```shell
    dpkg: error processing archive /var/cache/apt/archives/python-rospkg-modules_1.1.9-1_all.deb (--unpack):
     trying to overwrite '/usr/lib/python2.7/dist-packages/rospkg/__init__.py', which is also in package python-rospkg 1.1.4-1
    Errors were encountered while processing:
     /var/cache/apt/archives/python-rospkg-modules_1.1.9-1_all.deb
    E: Sub-process /usr/bin/dpkg returned an error code (1)
    ```

  * ```
    sudo dpkg -i --force-overwrite /var/cache/apt/archives/python-rospkg-modules_1.1.9-1_all.deb
    sudo apt-get -f install

#### 2.4 构建依赖

* ```
  sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

#### 2.5 测试是否安装成功

* ```
  roscore
  ```


* ```
  rosrun turtlesim turtlesim_node
  
* ```
  rosrun turtlesim turtle_teleop_key
  ```

* 可操纵小海龟移动即成功



## (三) 创建工作空间

### 1. 创建工作空间目录

* ```
  mkdir -p ~/ros_ws/src

### 2. 进入工作空间

* ```
  cd ~/ros_ws/

### 3. 初始化工作空间

* ```
  catkin_make

