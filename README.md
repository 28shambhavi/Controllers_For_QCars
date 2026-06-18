#Project Title: 
Controllers_For_QCars

#Summary: 
Developed trajectory-tracking controllers for two QCars integrated with the OptiTrack motion capture system in the Fluent Robotics Lab. Achieved synchronous tracking of given multi-robot trajectories at low speeds.

#Objective:
The objective of this project was to replicate the trajectory-tracking control previously implemented on MuSHR cars on the Quanser QCars.

The work began with understanding the ROS nodes, data transfer pipeline, motion capture system in the lab, and QCar setup. A feedback controller was first developed for a single car and tested at different speeds. The controller was then tuned to reduce trajectory-tracking error. After that, object-pushing experiments were conducted with a single car to evaluate payload capacity and its effect on motion. Finally, the control framework was extended to two cars using a central node for communication and synchronized tracking.

#System Setup:
The system setup involved configuring the Quanser QCar set up using the Quanser Academic Resources Repository and integrating it with the lab’s OptiTrack motion capture system. Additional setup support was taken from the lab’s internal motion capture documentation.

#Main Scripts:
combined_publisher.py
mpcspeed_steercontrol.py

#Trajectories tested:
Straight line
Circle
Lemniscate
Some other particular testing scenarios

#Tuning Summary:
Tuning process involved:
-prediction timestep and horizon length
-steering smoothness and aggressiveness
-state tracking weights
-speed tracking behaviour
-hardware-specific constraints and speed handling

#Outcome:
The project resulted in a tuned MPC pipeline for QCar trajectory tracking and established a structured workflow for simulation and hardware-based controller tuning.