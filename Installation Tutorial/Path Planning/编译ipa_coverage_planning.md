## 编译ipa_coverage_planning

### （〇) 环境

* Ubuntu18.04
* ROS-melodic



### （一) 编译

#### 1. 下载源码

* [GitHub - ipa320/ipa_coverage_planning: Algorithms for floor plan segmentation and systematic coverage driving patterns](https://github.com/ipa320/ipa_coverage_planning)

#### 2. 编译

* ```
  mkdir -p ~/pathplan_ws/src && cd ~/pathplan_ws
  ```

* ```
  catkin_make
  ```

* 将源码解压至~/pathplan_ws/src

* ```
  catkin_make
  ```

  * 若报错：Could not find the required component 'cob_map_accessibility_analysis'

    * 注释掉报错文件中所有存在Gurobi的语句

  * 若报错：No package 'coinutils' found

    * ```
      sudo apt-get install coinor-*

  * 若ipa_room_segmentation模块编译失败

    * ```
      # 在ipa_room_segmentation 模块的CmakeList.txt文件中加入
      
      add_compile_options(-std=c++11)

#### 3. 配置环境

* ```
  gedit ~/.bashrc
  
  # 添加下列内容
  
  source ~/pathplan_ws/devel/setup.bash
  ```

* ```
  source ~/.bashrc



### (二) 运行示例

#### 1. 启动服务端

* ```
  rosrun ipa_room_exploration room_exploration_server

#### 2. 启动用户端

* ```
  rosrun ipa_room_exploration room_exploration_client
  ```

#### 3. 通过lauch文件启动

* ```
  touch ~/pathplan_ws/src/ipa_coverage_planning-melodic_dev/ipa_room_exploration/ros/launch/room_exploration_action.launch
  ```

* ```html
  gedit ~/pathplan_ws/src/ipa_coverage_planning-melodic_dev/ipa_room_exploration/ros/launch/room_exploration_action.launch
  
  # 添加下列内容
  
  <?xml version="1.0"?>
  <launch>
  
  	<!-- room exploration server node -->
  	<node ns="room_exploration" pkg="ipa_room_exploration" type="room_exploration_server" name="room_exploration_server" output="screen" respawn="true" respawn_delay="2">
  	</node>
  
          <!-- room exploration client node -->
  	<node ns="room_exploration" pkg="ipa_room_exploration" type="room_exploration_client" name="room_exploration_client" output="screen">
                  <param name="image_path" type="string" value="/home/reallounger/pathplan_ws/src/ipa_coverage_planning-noetic_dev/ipa_room_segmentation/common/files/test_maps/lab_e.png"/>
  		<param name="exploration_algorithm" type="int" value="8"/>
                  <!-- this variable selects the algorithm for room exploration
                       1 = grid point explorator
                       2 = boustrophedon explorator
                       3 = neural network explorator
                       4 = convexSPP explorator
                       5 = flowNetwork explorator
                       6 = energyFunctional explorator
                       7 = voronoi explorator
                       8 = boustrophedon variant explorator-->
  	</node>
  
  	<!-- ACTIVATE IF NEEDED: run cob_map_accessibility_analysis_server -->
  	<!-- include file="$(find ipa_room_exploration)/ros/launch/cob_map_accessibility_analysis.launch"/-->
  
  	<!-- NOT NECESSARY ANYMORE: using direct library interface now, run coverage_check_server -->
  	<!-- include file="$(find ipa_room_exploration)/ros/launch/coverage_check_server.launch"/-->
  
  </launch>
  ```

* ```
  roslaunch ipa_room_exploration room_exploration_action.launch