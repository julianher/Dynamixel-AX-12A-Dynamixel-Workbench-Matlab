# Dynamixel-AX-12A-Dynamixel-Workbench-Matlab
## In this repository you will find a Matlab example script to make a ROS message and publish it to the controllers topic (/dynamixel_workbench/joint_trajectory) 

In this repository you will find a basic Matlab script example to control the dynamixel servos using the dynamixel workbench. This is done by making messages and publishing them to the /dynamixel_workbench/joint_trajectory topic. Besides you will also find a brief introduction to how to control the servo from the terminal

this repository is made with academic intentions to guide the students of the robotics course at the Universidad Nacional de Colombia.

## Prerequisites

Tested on **Ubuntu 16.04** with **ROS Melodic** full desktop installation. ( [ROS Installation](http://wiki.ros.org/melodic/Installation/Ubuntu))

* [Dynamixel Workbench](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_workbench/)   Do the download for ROS and if you already have a catkin_ws and you use catkin build don't forget to clone the package in the src and use catkin build instead.

## Setup
Tested with:
*  [USB2Dynamixel](https://emanual.robotis.com/docs/en/parts/interface/usb2dynamixel/)
*  [AX12-A](https://emanual.robotis.com/docs/en/dxl/ax/ax-12a/)

In case you have any problem related to de USB port try:

```bash
$ sudo chmod 777 /dev/ttyUSB0
```
Run this line to check your servo is conected

```bash
$ rosrun dynamixel_workbench_controllers find_dynamixel /dev/ttyUSB0
```

output: (This may take a while)
```bash
[ INFO] [1606708375.133052618]: Succeed to init(9600)
[ INFO] [1606708375.133082836]: Wait for scanning...
[ INFO] [1606708396.914114755]: Find 0 Dynamixels
[ INFO] [1606708396.914450658]: Succeed to init(57600)
[ INFO] [1606708396.914462484]: Wait for scanning...
[ INFO] [1606708414.947138819]: Find 0 Dynamixels
[ INFO] [1606708414.947538320]: Succeed to init(115200)
[ INFO] [1606708414.947551536]: Wait for scanning...
[ INFO] [1606708432.605411183]: Find 0 Dynamixels
[ INFO] [1606708432.605804044]: Succeed to init(1000000)
[ INFO] [1606708432.605826514]: Wait for scanning...
[ INFO] [1606708441.225827633]: Find 1 Dynamixels
[ INFO] [1606708441.225851225]: id : 1, model name : AX-12A
[ INFO] [1606708441.226307176]: Succeed to init(2000000)
[ INFO] [1606708441.226316773]: Wait for scanning...
[ INFO] [1606708458.530582724]: Find 0 Dynamixels
[ INFO] [1606708458.530859334]: Succeed to init(3000000)
[ INFO] [1606708458.530867632]: Wait for scanning...
[ INFO] [1606708475.828060092]: Find 0 Dynamixels
[ INFO] [1606708475.828423106]: Succeed to init(4000000)
[ INFO] [1606708475.828432877]: Wait for scanning...
[ INFO] [1606708493.122374825]: Find 0 Dynamixels

```

In this case we see I have a AX_12A with a 1000000 Bautrate


## Usage
First move the one_servo.yaml in to  /catkin_ws/src/dynamixel-workbench/dynamixel_workbench_controllers/config 

open the /catkin_ws/src/dynamixel-workbench/dynamixel_workbench_controllers/launch/dynamixel_controllers.launch 
* Set the Bautrate (In this case 1000000) 
* Change the basic.yaml file to one_servo.yaml.
```bash
<launch>
  <arg name="usb_port"                default="/dev/ttyUSB0"/>
  <arg name="dxl_baud_rate"           default="1000000"/>
  <arg name="namespace"               default="dynamixel_workbench"/>

  <arg name="use_moveit"              default="false"/>
  <arg name="use_joint_state"         default="true"/>
  <arg name="use_cmd_vel"             default="false"/>

  <param name="dynamixel_info"          value="$(find dynamixel_workbench_controllers)/config/one_servo.yaml"/>

  <node name="$(arg namespace)" pkg="dynamixel_workbench_controllers" type="dynamixel_workbench_controllers"
        required="true" output="screen" args="$(arg usb_port) $(arg dxl_baud_rate)">
    <param name="use_moveit"              value="$(arg use_moveit)"/>
    <param name="use_joint_states_topic"  value="$(arg use_joint_state)"/>
    <param name="use_cmd_vel_topic"       value="$(arg use_cmd_vel)"/>
    <rosparam>
      publish_period: 0.010
      dxl_read_period: 0.010
      dxl_write_period: 0.010
      mobile_robot_config:                <!--this values will be set when 'use_cmd_vel' is true-->
        seperation_between_wheels: 0.160  <!--default value is set by reference of TB3-->
        radius_of_wheel: 0.033            <!--default value is set by reference of TB3-->
    </rosparam>
  </node>
</launch>
```
Launch the controllers 


```bash
$ roslaunch dynamixel_workbench_controllers dynamixel_controllers.launch 
```
check you can move the servo using the terminal:

 Open a new terminal
 You can control the servo by using the next service:
 
 ```bash
$ rosservice call /dynamixel_workbench/dynamixel_command "command: ''
id: 1
addr_name: 'Goal_Position'
value: 200"
```
the AX_12A position is a 8bit register so you can modify it between 0-1023 

check the activ topics:

 ```bash
$ rostopic list"
```
output:

```bash
/dynamixel_workbench/dynamixel_state
/dynamixel_workbench/joint_states
/dynamixel_workbench/joint_trajectory
/rosout
/rosout_agg
```

Now subscirbe
open the /dynamixel_workbench/dynamixel_state

```bash
$ rostopic echo /dynamixel_workbench/dynamixel_state -n1
```
output:

```bash
  - 
    name: "servo"
    id: 1
    present_position: 198
    present_velocity: 0
    present_current: 0
---

```

Now open the AX_12 posicionAX12.mlx and run it. You can change the desire position but this time the position must be in radians and between (-2.6180,2.6180).



## Authors 

* **Julian Hernandez** - [julianher](https://github.com/julianher) - Initial Work 

UNIVERSIDAD NACIONAL DE COLOMBIA\
DEPARTAMENTO DE ING. MECANICA Y MECATRONICA \
ROBOTICA 2020-2\
PROF: PEDRO CARDENAS

AUTHOR: Julián Hernandez R\
Actuator: Dynamixel AX12\
MAIL: juahernandezro.edu.co\
VERSION: 1\
TESTED ON ROS Melodic (UBUNTU 18.04 LTS)\


