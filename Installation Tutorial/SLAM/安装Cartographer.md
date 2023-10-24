## 安装Cartographer

### （〇) 环境

* Ubuntu18.04
* ROS-melodic



### （一) 安装依赖

#### 1. 安装编译工具

* ```
  sudo apt-get update
  ```

  * 若报错：E: 仓库 “http://packages.ros.org/ros/ubuntu melodic” 没有 Release 文件。

  * 在[LUG's repo file generator (ustc.edu.cn)](https://mirrors.ustc.edu.cn/repogen/)找到对应版本最新的源

    * ```
      ## Ubuntu HTTPS IPv4 bionic(18.04)
      
      deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
      deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
      
      deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
      deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
      
      deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
      deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
      
      deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
      deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
      
      ## Not recommended
      # deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
      # deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

  * ```
    cd /etc/apt/sources.list.d
    ```

  * ```
    ls
    ```

  * ```
    # 若不存在ros-latest.list
    sudo mkdir ros-latest.list # 创建文件
    sudo chmod 777 ros-latest.list # 赋予文件权限
    ```

  * ```
    sudo gedit ros-latest.list
    ```

  * 将文件内容替换为上述找到的源并保存文件

  * ```
    sudo apt update
    ```

  * ```
    sudo apt upgrade
    ```

  * ```
    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
    ```

  * ```
    sudo gedit ros-latest.list
    
    # 应看到内容为
    # deb http://packages.ros.org/ros/ubuntu bionic main
    ```

  * ```
    sudo apt update

* ```
  sudo apt-get install -y python-wstool python-rosdep ninja-build stow

* 如果将系统环境python指向Anaconda安装的python3，则不需要安装python-wstool（Python3识别不到，Python2可以）——Python3也无法识别到python3-wstool

* 源码编译wstool

  * 下载源码：[wstool · PyPI](https://pypi.org/project/wstool/#history)

  * ```
    tar -xzvf wstool-0.1.17.tar.gz
    ```

  * ```
    cd ./wstool-0.1.17
    ```

  * ```
    # 打包成whl文件
    python setup.py sdist bdist_wheel || true
    ```

    * 若报错：/tmp/pip-req-build-rpp55ea6/setup.py:7: DeprecationWarning: the imp module is deprecated in favour of importlib; see the module's documentation for alternative uses

      * 报错原因：Python3用importlib代替了imp
      * 解决方案：报错文件仅用于查找版本号，直接对setup.py的version赋值即可

    * 若报错：WARNING: html_static_path entry '_static' does not exist（标红）

      * 报错原因：Sphinx库版本问题
      * 解决方案：安装最新版Sphinx或者选择合适版本的Sphinx

    * 若报错：SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.

      * 报错原因：setuptools库版本问题

      * 解决方案：安装不与ROS冲突的setuptools版本

      * ```
        pip install setuptools==58.2.0
        ```

  * ```
    cd ./dist
    ```

  * ```
    pip install wstool-0.1.17.whl
    ```

#### 2. 创建单独的工作空间

* ```
  mkdir carto_ws && cd carto_ws
  ```

* ```
  wstool init src
  ```

* ```
  wstool merge -t src https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall
  ```

* ```
  wstool update -t src

#### 3. 安装依赖

* ```
  sudo rosdep init
  ```

  * ```
    # 若失败则改国内源(rosdepc为rosdep国内版且自动运行第二条更新命令)
    
    sudo rosdepc init

* ```
  rosdep update
  ```

* ```
  rosdep install --from-paths src --ignore-src --rosdistro=melodic -y
  ```

  * 若报错：ERROR: the following packages/stacks could not have their rosdep keys resolved
    to system dependencies

    * ```
      rosdep install --from-paths ~/catkin_ws/src --ignore-src -r

  * 若仍报同样错

    * 解决方案：阅读后续报错，直接安装缺失的库

  * 若出现：#All required rosdeps installed successfully。则无需重复执行该命令，只安装缺失的库即可。

* 单独安装ceres-solver库

  * 下载源码：http://ceres-solver.org/ceres-solver-1.13.0.tar.gz

  * ```
    tar -xzvf ceres-solver-1.13.0.tar.gz
    ```

  * ```
    cd ceres-solver-1.13.0
    ```

  * ```
    mkdir build && cd build
    ```

  * ```
    cmake ..
    ```

    * 若报错：CMake 3.8 or higher is required. You are running cmake3.5.1

      * 下载源码：[Download | CMake](https://cmake.org/download/)

      * ```
        tar -xzvf cmake-3.26.3.tar.gz
        ```

      * ```
        # 删除原版本的cmake执行文件
        
        sudo rm /usr/bin/cmake
        ```

      * ```
        cd cmake-3.26.3
        ```

      * ```
        ./configure
        ```

      * ```
        make
        ```

      * ```
        sudo make install
        ```

  * ```
    make
    ```

    * 若报错：make: /usr/bin/cmake: Command not found

      * ```
        which cmake
        ```

      * ```
        # 若显示安装路径
        
        ln -s /usr/local/bin/cmake /usr/bin/cmake
        ```

  * ```
    sudo make install
    ```

* 单独安装abseil-cpp库

  * 下载的cartographer源码包中又absl的安装脚本

  * ```
    sudo apt-get install stow
    ```

  * ```
     sudo chmod +x ~/carto_ws/src/cartographer/scripts/install_abseil.sh
    ```

  * ```
    ~/carto_ws/src/cartographer/scripts/install_abseil.sh
    ```

* ```
  # 编译所需依赖
  
  sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-melodic-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev
  ```

* ```
  # 运行所需依赖
  
  sudo apt-get install ros-melodic-robot-state-publisher ros-melodic-pcl-conversions ros-melodic-pcl-msgs ros-melodic-pcl-ros
  ```



### (二) 编译与环境配置

#### 1. 编译

* ```
  catkin_make_isolated --install --use-ninja
  ```

* ```
  # 若存在下列报错,则删除工作空间，重复上述操作
  
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:67:58: error: expected ‘;’ at end of member declaration
     std::weak_ptr<Task> Schedule(std::unique_ptr<Task> task)
                                                            ^
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:68:22: error: ‘mutex_’ has not been declared
         LOCKS_EXCLUDED(mutex_) override;
                        ^~~~~~
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:68:30: error: ISO C++ forbids declaration of ‘LOCKS_EXCLUDED’ with no type [-fpermissive]
         LOCKS_EXCLUDED(mutex_) override;
                                ^~~~~~~~
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:73:46: error: expected ‘;’ at end of member declaration
     void NotifyDependenciesCompleted(Task* task) LOCKS_EXCLUDED(mutex_) override;
                                                ^
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:73:63: error: ‘mutex_’ has not been declared
     void NotifyDependenciesCompleted(Task* task) LOCKS_EXCLUDED(mutex_) override;
                                                                 ^~~~~~
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:73:71: error: ISO C++ forbids declaration of ‘LOCKS_EXCLUDED’ with no type [-fpermissive]
     void NotifyDependenciesCompleted(Task* task) LOCKS_EXCLUDED(mutex_) override;
                                                                         ^~~~~~~~
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:73:48: error: ‘int cartographer::common::ThreadPool::LOCKS_EXCLUDED(int)’ cannot be overloaded
     void NotifyDependenciesCompleted(Task* task) LOCKS_EXCLUDED(mutex_) override;
                                                  ^~~~~~~~~~~~~~
  /home/reallounger/carto_ws/src/cartographer/cartographer/common/thread_pool.h:68:7: error: with ‘int cartographer::common::ThreadPool::LOCKS_EXCLUDED(int)’
         LOCKS_EXCLUDED(mutex_) override;
         ^~~~~~~~~~~~~~

#### 2. 配置环境

* ```
  sudo gedit ~/.bashrc
  ```

* ```
  # 添加下列语句
  
  source ~/carto_ws/devel_isolated/setup.bash
  ```

* ```
  source ~/.bashrc



### (三) 运行示例程序

#### 1. 下载官方数据包

* https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag

#### 2. 保存文件（方便管理）

* 将下载的数据包放至~/carto_ws/src/cartographer_ros/bag中

#### 3. 建图

* ```
  roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=/home/reallounger/carto_ws/src/cartographer_ros/bag/cartographer_paper_deutsches_museum.bag
  ```

  * 若报错：check failure stack trace
    * 报错原因：绝对/相对路径格式问题；该命令需要绝对路径，且不允许用“~”代替“/home/reallounger”
    * 解决方案：修改路径格式

#### 4. 结束建图

* ```
  rosservice call /finish_trajectory 0
  ```

#### 5. 序列化保存当前状态

* ```
  rosservice call /write_state "{filename: '${HOME}/Downloads/b2-year-moon-day-hour-minute-second.bag.pbstream', include_unfinished_submaps: 'true'}"
  ```

  * 若报错：ERROR: Unable to send request. One of the fields has an incorrect type: field include_unfinished_submaps is not a bool

    * ```
      rosservice call /write_state "{filename: '${HOME}/Downloads/b2-year-moon-day-hour-minute-second.bag.pbstream', include_unfinished_submaps: 1}"

#### 6. pbstream--->pgm/yaml

* ```
  rosrun cartographer_ros cartographer_pbstream_to_ros_map -map_filestem=${HOME}/Downloads/b2-year-moon-day-hour-minute-second.bag -pbstream_filename=${HOME}/Downloads/b2-year-moon-day-hour-minute-second.bag.pbstream -resolution=0.05

