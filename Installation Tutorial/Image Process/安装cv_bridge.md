## 安装cv_bridge

### (一) 环境

#### 1. 系统

* Ubuntu18.04

#### 2. ROS

* Melodic

#### 3. Python

* Conda python3.8.8

#### 4. OpenCV

* OpenCV4.5.5



### (二) 安装

#### 1. 获取vision_opencv

* ```
  cd ~/ros_ws/src

* ```
  git clone https://gitee.com/irvingao/vision_opencv.git

#### 2. 编译

* ```
  cd ../

* ```
  export CPLUS_INCLUDE_PATH=/home/reallounger/anaconda3/include/python3.8

* ```
  catkin_make install -DCMAKE_BUILD_TYPE=Release -DSETUPTOOLS_DEB_LAYOUT=OFF -DPYTHON_EXECUTABLE=/home/reallounger/anaconda3/bin/python3.8 -DPYTHON_INCLUDE_DIR=/home/reallounger/anaconda3/include/python3.8 -DPYTHON_LIBRARY=/home/reallounger/anaconda3/lib/libpython3.8.so
  ```

  * 不指定路径会导致ROS默认使用Python2.7发生报错

#### 3. 添加环境变量

* ```
  gedit ~/.bashrc
  ```

* ```
  source ~/ros_cv_bridge/install/setup.bash --extend

* ```
  source ~/.bashrc

