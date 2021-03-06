# ardrone_cinvestav
This is the outcome of a research made in the Mechatronics section of the Electrical Engineering department at CINVESTAV.
This reporitory contains the source code for trajectory tracking control with AR.Drone 2.0 using ROS, Python and five different control laws. 
The repository contains 11 main .py files. 5 of these files are for single drone simulation using Gazebo, 5 more are for single drone real tests using an OptiTrack system and 1 more file is for simulating two drones using Gazebo and a control law based on a leader-follower scheme. 

## Documents folder
The papers consulted are in this folder. The type of control laws used includes:
- PD Controller
- Backstepping
- Adaptative Backstepping
- Discontinuous Control

## include folder
The folder contains the source code of a client socket. Since the Motive OptiTrack software is only available for Windows and ROS is only available for Linux, the data is sent via socket using TCP/IP. 
If you are interested in the source code for the server socket, visit my [optitrack_server](https://github.com/Misantonio/optitrack_server) repository. 

To run the client socket type
```
  roscore&
  rosrun ardrone_cinvestav publisherOpti
```

## src folder
This folder contains the source code. 
### lib folder
Contains third party Python modules.

**IMPORTANT:** In the constructor method of the `BasicDroneController` class in the `drone_controller.py` file, there is a modification that has to be done depending if the test to be done is a simulation or real one. 
The file contains the next lines of code.
```
  # Uncomment for simulation
  self.pubCommand = rospy.Publisher('/'+model+'/cmd_vel', Twist, queue_size=10)
  
  # Uncomment for real testing
  # self.pubCommand = rospy.Publisher('/cmd_vel', Twist, queue_size=10)
```
To perform simulations, uncomment `self.pubCommand = rospy.Publisher('/'+model+'/cmd_vel', Twist, queue_size=10)` and for real tests uncomment `self.pubCommand = rospy.Publisher('/cmd_vel', Twist, queue_size=10)`.
This was done because of how the ardrone_autonomy package is written. If any solution, contributors are welcome. 

### Results folder
In this folder is saved the outcome of each simulated or real test. 

## Installation
To install this repository, just clone it to your ROS workspace with
```
  roscd
  git clone https://github.com/Misantonio/ardrone_cinvestav.git
```
To work with these codes the ardrone_autonomy and the tum_simulator packages are necesssary. If yoy don't have these packages installed yet, type
```
  roscd
  git clone https://github.com/AutonomyLab/ardrone_autonomy.git
  git clone https://github.com/basti35/tum_simulator.git
  cd ..
  catkin_make
```

## Get Started
Let's test a PD controller in Gazebo. 
First, open the Gazebo simulator with the model of the AR.Drone 2.0
```
  roslaunch ardrone_cinvestav ardrone_testworld.launch
```
Once Gazebo has launched, run the simulation of the PD control law.
```
  rosrun ardrone_cinvestav sim_PD.py
```
The AR.Drone should take off and start to follow an oval trajectory.
