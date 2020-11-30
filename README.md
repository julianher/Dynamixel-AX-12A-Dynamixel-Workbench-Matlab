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
![find_dynamixel](https://ibb.co/qxRK9tG)

In this case we see I have a AX_12A with a 1000000 Bautrate


## Usage
First move the one_servo.yaml in to  /catkin_ws/src/dynamixel-workbench/dynamixel_workbench_controllers/config 

open the /catkin_ws/src/dynamixel-workbench/dynamixel_workbench_controllers/launch/dynamixel_controllers.launch 
* Set the Bautrate (In this case 1000000) 
* Change the basic.yaml file to one_servo.yaml.

![launch_file](https://ibb.co/fkH3hj0)

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

  UNIVERSIDAD NACIONAL DE COLOMBIA
  DEPARTAMENTO DE ING. MECANICA Y MECATRONICA
  ROBOTICA 2020-2
  PROF: PEDRO CARDENAS
  
  Publicación al tópico joint_trajectory ejemplo para posición.
  AUTHOR: Julián Hernandez R
  Actuator: Dynamixel AX12
  MAIL: juahernandezro.edu.co
  VERSION: 1
  TESTED ON ROS Melodic (UBUNTU 18.04 LTS)
  https://github.com/JurgenHK/2R-Manipulator-URDF

