# Super Mega Bot Path Planner

Package for path planning of Super Mega Bots for the ETH Robotics Summer School. 
The package has been tested under ROS Noetic and Ubuntu 20.04.

__Author__: Luca Bartolomei  
__Affiliation__: Vision For Robotics Lab, ETH Zurich  
__Contact__: Luca Bartolomei, lbartolomei@ethz.ch  

# Installation and Usage
To use the path-planning pipeline, refer to the [Wiki](https://github.com/VIS4ROB-lab/smb_path_planner/wiki). There, all the fundamental instructions are reported:
* [Installation and Documentation](https://github.com/VIS4ROB-lab/smb_path_planner/wiki/Installation-and-Documentation)
* [Running in simulation](https://github.com/VIS4ROB-lab/smb_path_planner/wiki/Running-in-simulation)
* [Running with real SMB](https://github.com/VIS4ROB-lab/smb_path_planner/wiki/Running-with-real-SMB)
* [Global map creation](https://github.com/VIS4ROB-lab/smb_path_planner/wiki/Global-map-creation)

To have a quick start, have a look at the [Cheatsheet](https://github.com/VIS4ROB-lab/smb_path_planner/wiki/Cheatsheet).

# Contributing
Contributions that help to improve the code are welcome. In case you want to contribute, please adapt to the [Google C++ coding style](https://google.github.io/styleguide/cppguide.html).

# Team8 setup guide:

```
* Simulation
** ~roslaunch smb_gazebo sim.launch~

* Navigation
** ~roslaunch smb_navigation navigate2d_ompl.launch sim:=true global_frame:=tracking_camera_odom follow_waypoints:=true use_global_map:=true~

* Map
** Obtain From Ilyass (mail)
** Put into =smb_path_planner/smb_navigation/data/mission=
** =map.yaml= and =map.pgm=

* Tune PID
** This is not super necessary right now
** run ~rosrun rqt_reconfigure rqt_reconfigure~
** control.gazebo_ros_control.pid_gains
*** Turn I for all components to roughly 60

* Publish challenge_frame
** ~rosrun tf static_transform_publisher 17 -21 0 -1.6 0 0 tracking_camera_odom challenge_frame 10~
** Angle still has to be slightly adjusted

* Publish waypoints
** Obtain =waypoints_out.csv=
** Place anywhere
** Run ~roslaunch smb_navigation_scripts follow_waypoints.launch input_filename:=$(pwd)/paths/waypoints_out.csv goal_frame_id:=challenge_frame~
** In another terminal, run ~rostopic pub /start_journey std_msgs/Empty -1~
```

# Run navigation stack on the robot

1- roslaunch smb smb.launch (on robot)
2- roslaunch smb_slam localization.launch map_name:=team7_mapdecimated.pcd (on robot)
3- roslaunch smb_navigation navigate2d_ompl.launch global_frame:=map use_global_map:=true (on robot)
4- roslaunch icp_localization icp_vis.launch (on host machine)
5- set 2D pose on the opened RVIZ on host machine
6- roslaunch smb_mission_planner mission_planner.launch config_file_path:=/home/helecomika/catkin_ws/src/smb_mission_planner/configs/mission_inside.yaml (on robot)
