# ROSI SIMULATION

This repository contains the simulated ROSI model and three real-world test scenarios.

The ROSI simulated model ROS programming interface is similar to the prototype, providing similar joint telemetry and accepting commands in the same topics. The model has been developed using `Coppelia v4.5.1 (rev. 2)`.


## Pictures

- ROSI model

![model](https://github.com/ITVRoC/rosi_sim/blob/main/resources/images/rosi.png)


- Test scenario: BMX course
  
![a1](https://github.com/ITVRoC/rosi_sim/blob/main/resources/images/a1.png)

- Test scenario: Rugged terrain

![a2](https://github.com/ITVRoC/rosi_sim/blob/main/resources/images/a2.png)

- Test scenario: roll and pitch courses

![a3](https://github.com/ITVRoC/rosi_sim/blob/main/resources/images/a3.png)


## Install

Software set:and `ROS Noetic` in `Ubuntu 20.04`.

1. [Download](https://www.coppeliarobotics.com/downloads) and install the CoppeliaSim simulator.

2. Clone to your `catkin_ws`, compile and source the [rosi_core](https://github.com/ITVRoC/rosi_core) and [rosi_modules](https://github.com/ITVRoC/rosi_modules) ROS packages, which provide required messages and services for the simulated model. The step-by-step on how compiling them can be found [here](https://github.com/ITVRoC/rosi_modules/wiki/Install)

3. Download and install to your `catkin_ws` the [CoppeliaSim ROS interface](https://github.com/CoppeliaRobotics/simROS). Follow the install instructions from their repository.

4. Go to the folder `catkin_ws/meta`. There you will find the two files: `messages.txt` and `services.txt`. Add the following lines to each one

   4.1. To `messages.txt`, add:
   ```
    rosi_common/DualQuaternionStamped
    controller/Control
   ```

   4.2. To `services.txt`, add:
   ```
    controller/RequestID
   ```

    4.3. Copy the most recently compiled sim_ros interface from `catkin_ws/devel/lib/libsimExtROS.so` to the CoppeliaSim folder.

That's it! The install process is done! Remember to re-source everything once new things are being compiled.


## Testing

1. Open a terminal and run `$ roscore`.

2. Open the CoppeliaSim simulator. You should be able to see the following initialization message in the terminal:
   ```
   [CoppeliaSim:loadinfo]   plugin 'ROS': loading...
   [CoppeliaSim:loadinfo]   plugin 'ROS': load succeeded.
   ```

3. Load the `rosi_sim/coppeliasim/robot/rosi.ttm` model.

4. Start the simulation by pressing the `play` button. Hope no yellow message appears in the Coppeliasim console.

5. Open a new terminal and input `$ rostopic list`. You should be able to see the folllowing topics:
```
/mti/sensor/imu
/rosi/cheat/pose_vel_rosi/dq
/rosi/cheat/pose_vel_rosi/twist
/rosi/rosi_controller/input
/rosi/rosi_controller/joint_state
/rosout
/rosout_agg
/sim/time
/tf
```
If you are seeing it, the installation procedure is OK :D. Now it's time to have fun!


## ROS topics

The ROSI model provide the following topics for interacting with the simulation:

- `/mti/sensor/imu` `<sensor_msgs/Imu>`: inertial data from the chassis, including orientation in unit quaternion format.

- `/rosi/cheat/pose_vel_rosi/dq` `<rosi_common/DualQuaternionStamped>`: the pose of the chassis wrt the world frame in dual quaternion format.

- `/rosi/cheat/pose_vel_rosi/twist` `<geometry_msgs/Twist>`: the twist speed of the chassis wrt the world frame.

- `/rosi/rosi_controller/input` `<controller/Control>`: topic that allows to send commands to the ROSI traction or flipper lever joints.

- `/rosi/rosi_controller/joint_state` `<sensor_msgs/JointState>`: provides the state of the model joints, as position, velocity and acceleration.

- `/sim/time` `<std_msgs/Header>`: the simulation time.






