# navigation 仿真

1. 安装ros仿真amcl插件，配置流程如下所示，

```
sudo apt-get install ros-kinetit-turtlebot-* 
sudo apt-get install ros-kinetit-arbotix-*

mkdir ~/catkin_ws
cd ~/catkin_ws/src
git clone https://github.com/pirobot/rbx1.git 
cd ..
catkin_make  
```

2. 启动仿真插件的流程，一切正常！

```
source ~/catkin_ws/devel/setup.bash           
roslaunch rbx1_bringup fake_turtlebot.launch

#新开一个终端
source ~/catkin_ws/devel/setup.bash  
roslaunch rbx1_nav fake_amcl.launch

#新开一个终端
rosrun rviz rviz 
find rbx1_nav`/nav.rviz
rosrun rqt_reconfigure rqt_reconfigure
```

3. 安装 ted_local_planner

~~~
cd ~/catkin_ws/src
git clone https://github.com/rst-tu-dortmund/teb_local_planner.git
git clone https://github.com/rst-tu-dortmund/teb_local_planner_tutorials.git
rosdep install teb_local_planner
cd ..
catkin_make 
~~~

4. 检测是否安装成功

~~~
rospack plugins --attrib=plugin nav_core 
~~~

 如果能查询到teb_local_planner，则表明安装成功。

5. 使用teb_local_planner：

```
roslaunch rbx1_bringup wb_fake_turtlebot.launch
roslaunch rbx1_nav wb_fake_amcl.launch
rviz 
find rbx1_nav/wb_test.rviz
rosrun rqt_reconfigure rqt_reconfigure
```



ros 官网：http://wiki.ros.org/teb_local_planner

安装参考网址: https://blog.csdn.net/xiekaikaibing/article/details/80197164

参数配置参考网址：https://www.ncnynl.com/archives/201708/1905.html



# stdr 超声波和避障仿真

1. 简介

   stdr_simulator 包实现了模拟机器人，和模拟传感器。到目前为止，实现了以下传感器：

   激光雷达,提供了 sensor_msgs/LaserScan消息类型；

   超声波传感器,提供sensor_msgs/Range消息类型；

   stdr_simulator包含：

   stdr_server:实现STDR模拟器同步和协调功能.     
   stdr_robot:提供机器人，传感器实现，使用nodelets为stdr_server加载它们。
   stdr_parser:为STDR模拟器提供一个库,以解析yaml和xml描述文件.
   stdr_gui:用于STDR模拟器中可视化的QT中的GUI.
   stdr_msgs:为STDR模拟器提供msgs、服务和操作。
   stdr_launchers:启动文件,方便启动服务器,机器人和gui
   stdr_resources:为STDR模拟器提供机器人和传感器提供描述文件
   stdr_samples:提供实例代码演示STDR功能.

2. 安装和使用:

   * apt-get安装

   ~~~
   sudo apt-get install ros-$ROS_DISTRO-stdr-simulator
   ~~~

   * git 安装

   ~~~
   git clone https://github.com/stdr-simulator-ros-pkg/stdr_simulator.git
   catkin_make
   ~~~

   * 运行

   ~~~
   roslaunch stdr_launchers server_with_map_and_gui_plus_robot.launch  // GUI
   roslaunch stdr_launchers rviz.launch          // RVIZ
   ~~~

   3.stdr仿真地图加载
   先启动一个无地图的服务器roslaunch stdr_launchers sever_no_map.launch
   用加载地图工具载入新地图
   roscd stdr_resources
   rosrun stdr_server load_map maps/sparse_obstacles.yaml(地图参数)
   4.生成一个已载入地图的服务
   roslaunch stdr_server server_with_map.launch
   5.机器人的操控
   使用robot_handler添加,删除,移动机器人
   6.添加机器人并指定姿态:$ roscd stdr_resources
   $ rosrun stdr_robot robot_handler add resources/robots/pandora_robot.yaml 9 7 1.57
   7.启动stdr图形界面
   roslaunch stdr_gui stdr_gui.launch
   8.移动机器人
   $ rosrun stdr_robot robot_handler replace /robot0 2 2 0
   9.删除机器人
   rosrun stdr_robot robot_handler delete /robot0

   10.用XML文件创建激光传感器
   在stdr_recource包中的hokuyo文件夹创建.xml文件
   11.从GUI工具界面加载地图,创建机器人:
   先启动一个无地图服务,再启动GUI界面,点击载入地图或创建机器人
   使用鼠标与机器人交互右键机器人可选择删除,移动跟随机器人.

ros 官网：http://wiki.ros.org/stdr_simulator

