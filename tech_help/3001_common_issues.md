# RBE 3001 Common Setup Issues

This guide is intended to cover common issues encountered while setting up your development environment for RBE3001. 
### A few things to check first:
1.  Is your robot turned on?
2. Did you run the robot.m file/is the Robot object visible in the workspace?
3. Is your path set correctly in MATLAB?
### 1. Dynamixel SDK
___
If you are getting the error message:
```
Could not find file dynamixel_sdk.h.

Error in OM X arm (line 33)
		[notfound, warnings] = ...
```
there is a chance the Dynamixel SDK library was either not built correctly or is in the wrong folder, or this is another [[RBE 3001 Tech Guide#b) Path is not set correctly| MATLAB path issue.]]

if the library was not built correctly, either go into the directory `c/build/linux64` and run `make clean && make` or remove the SDK by being in its parent directory and 
### 2. Serial/USB Port
---
A connection issue to the robot is usually due to one or a combination of the following three things:
- the robot is not turned on
- faulty USB cable/robot USB port
- USB port permissions are incorrect

The fix for reason #1 should be fairly obvious, a faulty USB port/cable should be reported to the Lab Assistants as soon as possible either by notifying a Student Assistant directly or sending a message in the RBE 300n Discord channel if there are no office hours.

**Port permissions can be corrected by running the command:** `sudo chmod a+rw /dev/ttyUSB0`

If you are still struggling with the robot/serial port being recognized, it is advised to switch to a Lab PC as the port permissions should be already configured.
### 3. MATLAB
---
##### a) GUI Scaling Issues
Check the Ask Ubuntu forum link:
https://askubuntu.com/questions/1478596/matlab-incomplete-scaling-ignoring-system-scaling
##### b) Path is not set correctly
Typical error message:
```
Error using loadLibrary
Could not find file dynamixel_sdk.h
```
 Please refer to the pre-lab doc, namely the second numbered link for how to resolve this issue.
In the future the `addpath( )` function can be used to directly add certain directories to the MATLAB path can can be used in scripts.
##### c)  Robot.m file was not run
Typical error message:
```
Error in Robot

Error in {file you are trying to run} (line ##)
robot = Robot(); % creates robot object
        ^^^^^^^
```

This means that the robot object was never created in the workspace. Check the side panel titled "workspace" to see if a robot object exists.


</br>
---
</br>
Last edited by: K. Herbstzuber - A25