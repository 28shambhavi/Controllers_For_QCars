#Qcar Set Up, Issues, and Fixes

#QCar Setup

The QCar setup can be completed using the official Quanser Academic Resources repository, which provides documentation for hardware setup, software installation, and system configuration.

Some of the setup issues encountered during this project, along with their fixes and useful commands, are documented below.

#Network and Lab Configuration

>Lab Network Channel
The Wi-Fi channel on the lab network may need to be adjusted for the QCars, as interference can affect communication with other robots on the same network.

>NatNet ROS GUI Configuration
For the desktop system used with the QCar, the following OptiTrack/NatNet ROS settings were used:
Server IP: 192.168.1.166
Client IP: 192.168.1.111
Multicast Address: 239.255.42.101

#QCar Subscriber Script
A script was created on the QCar for subscribing and running the controller: ./run_car.sh

#Python and ROS Issues on the QCar: Some issues were encountered with Python libraries and ROS dependencies on the QCar. These were resolved by reinstalling required packages, rebuilding the ROS workspace, and reconfiguring Python library paths.

#Wi-Fi Troubleshooting: Several connectivity issues were encountered with the lab Wi-Fi network. The following commands were useful for diagnosing and fixing these issues.

>Disconnect a Particular Interface
sudo nmcli device disconnect wlan0

>Bring Down an Unwanted Auto-Connected Network
sudo nmcli connection down "MWireless [0ffce0d7]"


#QCar Bridge Setup

Bridge Script Location

>The target bridge script was placed at:
~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py

>Create the Catkin Workspace
cd ~
mkdir -p ~/my_qcar_ws/src
cd ~/my_qcar_ws/src
source /opt/ros/melodic/setup.bash

>Create the ROS Package
catkin_create_pkg my_qcar_bridge_pkg rospy ackermann_msgs

This creates the package at:
~/my_qcar_ws/src/my_qcar_bridge_pkg

>Create the Scripts Folder
mkdir -p ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts
cd ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts

#Create the Bridge Script

>Create and edit the Python bridge script:
vim my_qcar_bridge.py
Paste the bridge code into the file and save it.

>Make the Script Executable
chmod +x ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py

>Error Encountered
chmod: cannot access '/home/nvidia/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py': No such file or directory

Cause: The workspace, package, or script had not yet been created.

Fix: Create the workspace, package, and script in the correct order before attempting to modify permissions.

>Build the Workspace
cd ~/my_qcar_ws
source /opt/ros/melodic/setup.bash
catkin_make
source ~/my_qcar_ws/devel/setup.bash


#OptiTrack / ROS / Ackermann Interface
The OptiTrack setup can be completed using the lab’s internal motion capture documentation. The network parameters used for NatNet/OptiTrack communication are listed above.

#Software Flow
The software workflow used in this project was:

>Establish open-loop control to confirm connection with the QCars
>Set up the OptiTrack motion capture system for the QCar
>Run the trajectory-tracking controller
>Tune the controller
>Perform payload experiments

#Script Descriptions

>combined_publisher.py — publisher script for running both cars together
>live_plotter.py — live plotting script for visualizing QCar trajectories
>mpcspeed_steercontrol.py — MPC controller for the QCars
>qcar_params.py — parameter definitions for the QCars
>trajectory.py — trajectory generation script for available path options

#During experiments, only the following scripts needed to be run directly:

>combined_publisher.py
>mpcspeed_steercontrol.py

#Motion Capture Issues with Two QCars
A challenge encountered while running both QCars together was that the motion capture system sometimes struggled to differentiate between the cars.

#Fixes Implemented
>placed markers at different positions on the two cars
>used different numbers of markers where needed
>enabled the minimum-marker-detection option in OptiTrack

#Steering Offset
Both QCars exhibited an approximate steering offset of about 7 degrees. This was compensated for in the code using steer_trim.

#Debugging Issues Encountered During Tuning
Several debugging issues were encountered and addressed during controller tuning, the debugging comments are added in the scripts. Some of the extra debugging issues resolved were:
>The coordinate system between the qcar and the mocap had to be checked (Do set the transformation in the optitrack)
>state estimation issues related to v and yaw
>quaternion formatting issues
>trajectory alignment issues, particularly between the path center and the car’s starting point

The trajectory alignment can be adjusted by the user in the trajectory script depending on the desired experimental setup.


























#The Channel of the Lab network needs to be adjusted for the qcars as there are some interruptions caused on other robots.

#The natnet_ros_cpp Gui asks for the client ip and the server ip, for the desktop system in use for the qcar the following are the details:
Server ip: 192.168.1.166
Client ip: 192.168.1.111 
Multicast Address: 239.255.42.101

#For subscribing the following file on the qcar was be created:
./run_car.sh

#Some issues occured in the usage of python libraries and ROS on QCar, those issue were resolved by reinstalling a few things and set up Python libraries again.

#A lot of wi fi issues were faced regarding the lab wi fi network, following steps can we used to figure those problems out:
>To check the network connection:
nmcli device status

>To connect to fluent-5 wi-fi
sudo nmcli device wifi connect  "fluent-5" password "This-Network-Is-For-Robot-Use-Only"

>Active connections
nmcli connection show --active

>To disconnect a particular connection
sudo nmcli device disconnect wlan0

>Forceful connection down if automatically getting connected to another wi fi
sudo nmcli connection down "MWireless [0ffce0d7]"     

>QCar Bridge Setup:
The target script location is:
~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py

>Created Catkin Workspace
cd ~
mkdir -p ~/my_qcar_ws/src
cd ~/my_qcar_ws/src
Source ROS Melodic:
source /opt/ros/melodic/setup.bash
>Created the ROS Package
catkin_create_pkg my_qcar_bridge_pkg rospy ackermann_msgs
This creates the package here:
~/my_qcar_ws/src/my_qcar_bridge_pkg
>Created the Scripts Folder
mkdir -p ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts
cd ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts
>Created the Bridge Script
Created the Python bridge script.
vim my_qcar_bridge.py
Pasted the bridge code into this file.
The final path:
~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py
>Save and Exit Vim
After pasting the code:
Esc
:wq
Enter
>Make the Script Executable
Give executable permission to the script.
chmod +x ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py
Error:
chmod: cannot access '/home/nvidia/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py': No such file or directory
happened because the workspace/package/script had not been created yet.
>Build the Workspace
Go to the workspace root and build it.
cd ~/my_qcar_ws
source /opt/ros/melodic/setup.bash
catkin_make
Then source the workspace:
source ~/my_qcar_ws/devel/setup.bash

>Check
.bashrc
To see what has already been added to the bash configuration file:
vim ~/.bashrc
or for easier viewing:
nano ~/.bashrc
To only view without editing:
cat ~/.bashrc
For a long file:
less ~/.bashrc
Press q to exit less.
To specifically search for ROS/QCar-related lines:
grep -n "ROS\|qcar\|QCar\|PYTHONPATH\|source" ~/.bashrc
Useful lines that may be added to ~/.bashrc:
source /opt/ros/melodic/setup.bash
source ~/my_qcar_ws/devel/setup.bash
export PYTHONPATH=/home/nvidia/Quanser/libraries/python:$PYTHONPATH
For ROS networking, also add the correct IPs:
export ROS_MASTER_URI=http://<ROS_MASTER_IP>:11311
export ROS_IP=<THIS_QCAR_IP>
After editing .bashrc, reload it:
source ~/.bashrc

>Set ROS Network Variables
Check the current QCar IP:
hostname -I
Then set:
export ROS_MASTER_URI=http://<ROS_MASTER_IP>:11311
export ROS_IP=<THIS_QCAR_IP>
Example:
export ROS_MASTER_URI=http://192.168.1.121:11311
export ROS_IP=192.168.1.130
Here:
ROS_MASTER_URI
points to the machine running roscore.
ROS_IP
should be the IP address of the current QCar.

>Run the Bridge
Run the bridge using:
cd ~/my_qcar_ws
source /opt/ros/melodic/setup.bash
source ~/my_qcar_ws/devel/setup.bash
PYTHONPATH="/home/nvidia/Quanser/libraries/python:$PYTHONPATH" rosrun my_qcar_bridge_pkg my_qcar_bridge.py
If hardware access requires sudo, run:
sudo env \
ROS_MASTER_URI=$ROS_MASTER_URI \
ROS_IP=$ROS_IP \
PYTHONPATH="/home/nvidia/Quanser/libraries/python:$PYTHONPATH" \
/usr/bin/python3 ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py

>Debugging Error:
No module named ackermann_msgs.msg
The error was:
Traceback (most recent call last):
  File "/home/nvidia/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py", line 5, in <module>
    from ackermann_msgs.msg import AckermannDriveStamped
ImportError: No module named ackermann_msgs.msg
This means Python could not find the ROS ackermann_msgs package.
Possible causes:
ackermann_msgs is not installed.
The workspace was not built or sourced correctly.
The script is being run with Python 3 while ROS Melodic packages are available only for Python 2.

>Check if
ackermann_msgs
Exists
Run:
source /opt/ros/melodic/setup.bash
rospack find ackermann_msgs
If installed correctly, it should return something like:
/opt/ros/melodic/share/ackermann_msgs
If not found, install it:
sudo apt-get update
sudo apt-get install ros-melodic-ackermann-msgs
Then check again:
rospack find ackermann_msgs

>Rebuild After Installing Dependencies
After installing ackermann_msgs, rebuild the workspace.
cd ~/my_qcar_ws
source /opt/ros/melodic/setup.bash
catkin_make
source ~/my_qcar_ws/devel/setup.bash

>Test Python Imports
Test whether ackermann_msgs works in Python 2:
python -c "from ackermann_msgs.msg import AckermannDriveStamped; print('ackermann import works')"
Test whether it works in Python 3:
python3 -c "from ackermann_msgs.msg import AckermannDriveStamped; print('ackermann import works')"
If Python 2 works but Python 3 fails, the problem is that ROS Melodic is using Python 2 message packages, while the script is being run with Python 3.

>Check the Script Shebang
Open the bridge script:
vim ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py
The first line should be:
#!/usr/bin/env python
Not:
#!/usr/bin/env python3
For ROS Melodic, #!/usr/bin/env python is usually safer because Melodic is Python 2-based by default.

>Test PAL Import
Since the QCar hardware interface uses the Quanser PAL library, also test:
python -c "from pal.products.qcar import QCar; print('pal ok')"
and:
python3 -c "from pal.products.qcar import QCar; print('pal ok')"
This helps identify whether PAL works with Python 2 or Python 3.

>Main Issue to Watch For
There may be a Python version conflict:
ROS Melodic ackermann_msgs may work in Python 2.
Quanser PAL may work in Python 3.
If that happens, the bridge may need to be adjusted depending on which Python version both ROS messages and PAL can support.

#Clean Full Command Sequence in short
Use this as the complete setup sequence for a new QCar:
cd ~
mkdir -p ~/my_qcar_ws/src
cd ~/my_qcar_ws/src
source /opt/ros/melodic/setup.bash
catkin_create_pkg my_qcar_bridge_pkg rospy ackermann_msgs
mkdir -p ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts
vim ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py
chmod +x ~/my_qcar_ws/src/my_qcar_bridge_pkg/scripts/my_qcar_bridge.py
cd ~/my_qcar_ws
catkin_make
source devel/setup.bash
PYTHONPATH="/home/nvidia/Quanser/libraries/python:$PYTHONPATH" rosrun my_qcar_bridge_pkg my_qcar_bridge.py

#Useful Checks Before Running
Check available Ackermann topics:
rostopic list | grep ackermann
Check message type:
rostopic typ e:
ackermann_msgs/AckermannDriveStamped
Check message definition:
rosmsg show ackermann_msgs/AckermannDriveStamped

#OptiTrack / ROS / Ackermann interface:
OptiTrack set up can be done with the help of Optitrack google doc of the lab. The ip addresses are mention above.

#Software flow:
>Open loop control first to establish the connection to the qcars.

>Then set up of the optitrack motion capture system for the qcar.

>The controller used to do trajectory control.

>Tune the Controller.

>Set up for the payload tests.

#Which script does what:
>combined_pubisher.py - The publisher script for running both the cars together.
>live_plotter.py - Plotting the live plotter for the trajectory of the qcar live.
>mpcspeed_steercontrol.py - the mpc controller for the qcars.
>qcar_params.py - contains all the parameters used for the qcars
>trajectory.py - The trajectory scripts for all the options.

>Only combined_publisher.py and mpcspeed_steercontrol.py scripts need to be run during a experiment.

#Mocap with two qcars:
There was problem faced when running both the qcars togetehr as mocap is not able to differentiate between cars:
>Placing the markers at different positions.
>Using different number of markers.
>Choosing the minimum marker detection option in the optitrack.

#The offset value of the angles
>Both the cars have some offset angle value approx 7 degrees for which they  have been adjusted in the code by usin gsteer_trim.

#While Tuning some of the debugging issues which were resolved:
>The states v and yaw
>The Quaternion formatting
>The points in the trajectory like the center of the trajectory vs. the car's starting point with respect to that. It can be changed as required yb bthe user in the trajectory script.
