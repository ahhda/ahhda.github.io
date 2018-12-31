---
title: Running Autonomous Hector SLAM on Lizi
date: 2017-02-10 19:22:00 +05:30
categories: [ros, SLAM, self driving]
layout: post
author: anuj
image: assets/images/gazebo.png
---

To run a simple navigation/exploration demo of an unknown map there is a pretty stable SLAM implementation known as [Hector Slam](http://wiki.ros.org/hector_slam). You can run hector SLAM with [hector navigation](http://wiki.ros.org/hector_navigation) package for autonomous driving. The package looks for unexplored frontiers and sends the robot to explore that area.

You can find the code for Hector navigation on [github](https://github.com/tu-darmstadt-ros-pkg/hector_navigation).

To run hector SLAM with autonomous driving here are the steps:

* Clone the repo.
  
```
git clone https://github.com/tu-darmstadt-ros-pkg/hector_navigation.git
```

* Create a workspace for e.g. catkin_ws and put the contents in the src folder. Then run:  

```
catkin_make
```

* Now activate your bot with sensors. For example for a Lizi Robot you do something like:

```
roslaunch robotican_lizi lizi.launch hector_slam:=true move_base:=true lidar:=true world_name:="`rospack find robotican_common`/worlds/building.sdf" gazebo:=true
```

A gazebo window with the robot and map would open up.
![gazebo.png]({{ site.baseurl }}/assets/images/gazebo.png)

* Now fire up Rviz:

```
rosrun rviz rviz -d `rospack find robotican_demos`/nfig/hector_slam.rviz
```

![rviz.png]({{ site.baseurl }}/assets/images/rviz.png)

* Finally run the hector exploration node followed by the hector exploration controller.

```
roslaunch hector_exploration_node exploration_planner.launch
rosrun hector_exploration_controller simple_exploration_controller
```

You should now have your bot navigating and building the map for you.
