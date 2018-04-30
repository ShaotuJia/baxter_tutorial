# ENPM 661 Project 4 Baxter Tutorial
## Brief
This project uses robot Baxter to pick and place a box on a table. During the movement of the baxter arm, it need to avoid an obstacle that placed on the same table. 
## Author
- Shaotu Jia
- Zejiang Zeng
## Package Dependency
- Baxter Simulator
- IK Solver
- MoveIt!
- Path Planner (OMPL)
## How to Run
- before run this package, make sure you already install the essential packages for baxter. [install](http://sdk.rethinkrobotics.com/wiki/Workstation_Setup#Step_4:_Install_Baxter_SDK_Dependencies)
- clone this package to your ros workspace. e.g ~/catkin_ws
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/ShaotuJia/baxter_tutorial.git
```
- compile packages by catkin_make 
```
$ cd ~/catkin_ws
$ catkin_make
```
- make the script file to executable
```
$ cd ~/catkin_ws/src/baxter_tutorial/scripts
$ chmod +x ik_pick_and_place_demo.py
```
- run gazebo simulator for baxter
```
$ ./baxter.sh sim
$ roslaunch baxter_gazebo baxter_world.launch
```
- run pick and place demo in this package
```
$ ./baxter.sh sim
$ rosrun baxter_tutorial ik_pick_and_place_demo.py
```

## Detail
This package baxter_tutorial is based on baxter_sim_examples/ik_pick_and_place_demo.py. The following is the changes in the script.
- change the position of block[1] to place box on a different position. (line 314)
- add function 'move_arm' to move the end effector of left arm to desired position. (line 144 ~ 147)
- add a list 'waypoints' to store the trajectory of arm for avoiding collision. (line 322 ~ 328)
- call move_arm function to move the left arm following the trajectory. (line 335 ~ 340)
- remove the while loop. So this script only pick and place once. (line 349 ~ 358)

The obstacle is based on the baxter_pillar. Since the the defalut obstacle in baxter_pillar is too large to avoid, it is changed to a small box on the table (baxter_box). 

## MoveIt! Path Plan
To get the waypoints in the script, we need Moveit package to the trajectory without collision. 
### How to Run MoveIt! for Baxter
First, run baxter gazebo simulator
```
$ ./baxter.sh sim
$ roslaunch baxter_gazebo baxter_world.launch
```
And then, if you want to use the model and start position in ik_pick_and_place_demo.py, you can comment out line 330 ~ 345 and run it.
Next, run baxter joint controller
```
$ ./baxter.sh sim
$ rosrun baxter_interface joint_trajectory_action_server.py
```
Finally, start MoveIt! RVIZ
```
$ ./baxter.sh sim
$ roslaunch baxter_moveit_config demo_baxter.launch right_electric_gripper:=true left_electric_gripper:=true
```
### How to Use MoveIt
- import scenes/baxter_box.scene to RVIZ
![alt text](https://github.com/ShaotuJia/baxter_tutorial/blob/master/moveit_process/start.png)
- Drag the arm to desired position and make a plan. Hint: the draged position should be a little offset to original position. Otherwise, the planner will fail to make a plan. 
![alt text](https://github.com/ShaotuJia/baxter_tutorial/blob/master/moveit_process/Plan.png)
- Execute the plan
![alt text](https://github.com/ShaotuJia/baxter_tutorial/blob/master/moveit_process/finish.png)
- Use ROS tf to find the current position of gripper
```
$ rosrun tf tf_echo /world /left_gripper
```
![alt text](https://github.com/ShaotuJia/baxter_tutorial/blob/master/moveit_process/tf_echo.png)



