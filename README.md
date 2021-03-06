# About
This project has an end-to-end implementation (from simulation to actual deployment on hardware) for a ground rover (attacker), more technically called as a UGV (Unmanned Ground Vehicle), to avoid another ground rover (defender) and reach a goal position. Involves building the rover, path planning, path following, writting the controller, testing in simulation and finally deployment on a real ground rover.
The project has various phases:
* Building a rover using the available hardware and drive manually
* Deployment of autopilot system on the rover and able to do autonomous missions
* Setup of ArduRover SITL simulator and making a connection with the ROS system using mavros
* Testing of controller and tune the PID gains so that the rover is able to follow a set of waypoints (in simulation)
* Testing of the path planning algorithm to avoid the defender rover and reach the goal position (in simulation)
* Finally testing the entire system on a real ground rover

## Problem Scenario
The attacker bot has to avoid the defender bot and reach a pre-determined goal position. The bots have access of each other's GPS location. (The goal of the defender is touch the attacker bot, thus avoiding it to reach the goal position)

# Hardware Building
The below shows the various parts used to build the bot i.e. chasis, tires, battery etc.
![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180227_113330963.jpg) <br/>
<br/>

The motors being attached on the sides:
![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180227_141142109.jpg) <br/>
<br/>


The motors and battery (including the switch) being attached to the motor driver (The motor driver being used is Sabertooth 2x12):
![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180313_091100959.jpg) <br/>
<br/>

The receiver being attached to the above setup:
![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180313_091128471.jpg) <br/>
<br/>

The motor driver is being operated in **differential drive** mode. Below shows a video of the rover being operated in manual mode:
[![Manual Video](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/Screen%20Shot%202018-06-02%20at%207.10.25%20PM.png)](https://www.youtube.com/watch?v=ZhnLZOoGwi0 "Manual Video")
<br/>
<br/>

## Autopilot and RPi Installation
The autopilot (Pixhawk) is setup on the rover. It draws power from the Li-ion battery (blue colour), which also powers the motors. RPi is also deployed which is being powered by the power bank (white colour). The ROS system runs on the RPi which sends commands to the autopilot to control the rover. ArduPilot documentation for first-time autopilot setup: http://ardupilot.org/rover/docs/apmrover-setup.html <br/>

![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180418_195946615.jpg) <br/>
<br/>

In order to safegaurd the compass from the magnetic interference of motors and wires, the compass module was raised from the base as shown below:
![alt text](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/IMG_20180429_154743374.jpg) <br/>
<br/>

 
# Simulation Testing
The SITL simulation of ArduRover can be setup by following this project: https://github.com/adityajain07/SITL-Simulation_Ardurover_P-Controller. It includes instructions to set up the simulation, writing your controller and tuning the PID gains.

## Path Planning Algorithm
In order to avoid the defender bot and at the same time reach the goal position, the concept of potential fields was applied. Read more about it: https://pdfs.semanticscholar.org/725e/fa1af22f41dcbecd8bd445ea82679a6eb7c6.pdf. The code can be found in 'Potential_Fields.py'. The goal position can be defined in 'waypoints.txt' and obstacle position is to be mentioned in the python script itself.
<br/>
<br/>
Below shows the simulation for the same:<br/>
[![Manual Video](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/pot_field.png)](https://www.youtube.com/watch?v=C4m1UPOL8Ak "Manual Video")


# Deployment on Ground Rover
The controller, path planning and path following was then tested on the real rover. Some points to note:

* In case you face geolibrary issue after installing ROS on RPi: (this library needs to be installed manually): https://github.com/mavlink/mavros/blob/master/mavros/README.md

* Setting permission for the USB port (so that the RPi can give commands to the Pixhawk via the USB cable): sudo chmod 666 /dev/ttyACM0

* Command for connecting your RPi (with ROS running) to the autopilot: roslaunch mavros apm2.launch fcu_url:=serial:///dev/ttyACM0:921600

Below video shows the rover running with all the systems deployed:
[![Manual Video](https://github.com/adityajain07/Attacker-Rover_Hardware_Software/blob/master/Photos/bot.png)](https://youtu.be/UdvXVk8Mf14 "Manual Video")
