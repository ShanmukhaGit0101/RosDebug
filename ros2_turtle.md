# Hands-on with TurtleBot3

### ROS 2 Jazzy + Ubuntu 24.04

**Workshop:** Hands-on with TurtleBot3
**ROS 2 Distribution:** Jazzy Jalisco
**Ubuntu:** 24.04 LTS
**Robot:** TurtleBot3 Burger
**Simulation:** Gazebo

---

# 1. Overview

This workshop introduces TurtleBot3 simulation and navigation using ROS 2 Jazzy.
---

# 2. Prerequisites

Before starting, make sure you have:

* Ubuntu 24.04 LTS
* ROS 2 Jazzy installed
* Internet connection
* Git
* `colcon`
* Gazebo / Gazebo ROS integration
* RViz2

Check your ROS 2 installation:

```bash
source /opt/ros/jazzy/setup.bash
```

Check the ROS 2 distribution:

```bash
echo $ROS_DISTRO
```

Expected:

```text
jazzy
```

Check ROS 2:

```bash
ros2 --version
```

---

# 3. Prepare the ROS 2 Workspace

Create a workspace:

```bash
mkdir -p ~/ros2_ws/src
```

Go to the workspace:

```bash
cd ~/ros2_ws/src
```

Source ROS 2 Jazzy:

```bash
source /opt/ros/jazzy/setup.bash
```

The workspace will have the following structure:

```text
~/ros2_ws/
└── src/
```

All TurtleBot3 source repositories will be placed inside:

```text
~/ros2_ws/src
```

---

# 4. Install Required Dependencies

Update the package index:

```bash
sudo apt update
```

Install commonly required ROS 2 packages:

```bash
sudo apt install -y \
  python3-colcon-common-extensions \
  python3-rosdep \
  python3-vcstool \
  git
```

Initialize `rosdep` if it has not already been initialized:

```bash
sudo rosdep init
```

If it reports that rosdep is already initialized, that is fine.

Update rosdep:

```bash
rosdep update
```

---

# 5. Install TurtleBot3

## Step 1 — Clone TurtleBot3 Repositories

Go to the source directory:

```bash
cd ~/ros2_ws/src
```

Clone the TurtleBot3 repositories.

### TurtleBot3 Core

```bash
git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3.git
```

### TurtleBot3 Messages

```bash
git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
```

### TurtleBot3 Simulations

```bash
git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
```

Check the repositories:

```bash
ls
```

You should see:

```text
turtlebot3
turtlebot3_msgs
turtlebot3_simulations
```

---

# 6. Install Package Dependencies

Go back to the workspace:

```bash
cd ~/ros2_ws
```

Install dependencies:

```bash
rosdep install --from-paths src --ignore-src -r -y
```

If rosdep reports that some packages are already installed, that is normal.

---

# 7. Build the Workspace

Build all packages:

```bash
cd ~/ros2_ws
colcon build --symlink-install
```

After a successful build, source the workspace:

```bash
source ~/ros2_ws/install/setup.bash
```

Verify TurtleBot3 packages:

```bash
ros2 pkg list | grep turtlebot3
```

You should see packages such as:

```text
turtlebot3
turtlebot3_bringup
turtlebot3_description
turtlebot3_gazebo
turtlebot3_navigation2
turtlebot3_node
turtlebot3_msgs
```

---

# 8. Source the Workspace in Every Terminal

Every new terminal should have both ROS 2 and the workspace sourced:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

You can add these commands to `.bashrc`:

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

Reload:

```bash
source ~/.bashrc
```

> If you use multiple ROS 2 workspaces, be careful about sourcing them in the wrong order.

---

# 9. ROS 2 Launch

## What is a Launch File?

A ROS 2 launch file allows multiple nodes and configurations to be started using a single command.

Without a launch file, you may need several terminals:

```bash
ros2 run package1 node1
```

```bash
ros2 run package2 node2
```

```bash
ros2 run package3 node3
```

A launch file allows these components to be started together.

---

# 10. Simple ROS 2 Launch Example

Create a Python launch file:

```text
my_launch.py
```

Example:

```python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():

    return LaunchDescription([

        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='turtle1',
            output='screen'
        ),

        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='turtle2',
            output='screen'
        ),

    ])
```

The important parts are:

### Import

```python
from launch import LaunchDescription
```

This provides the ROS 2 launch description.

### Node

```python
from launch_ros.actions import Node
```

This allows ROS 2 nodes to be launched.

### Required function

Every Python launch file should contain:

```python
def generate_launch_description():
```

The function returns:

```python
LaunchDescription([...])
```

---

# 11. Launch File Node Configuration

A typical node configuration looks like:

```python
Node(
    package='turtlesim',
    executable='turtlesim_node',
    name='turtle1',
    output='screen'
)
```

### Parameters

| Parameter    | Purpose                                 |
| ------------ | --------------------------------------- |
| `package`    | ROS 2 package containing the executable |
| `executable` | Node executable                         |
| `name`       | Name assigned to the node               |
| `output`     | Where node output is displayed          |

---

# 12. Installing Launch Files in a Python Package

For an `ament_python` package, launch files should be installed as part of the package.

In `setup.py`, include:

```python
from glob import glob
import os
```

Then add the launch files to `data_files`:

```python
(os.path.join('share', package_name, 'launch'), glob('launch/*.py')),
```

This allows ROS 2 to find the launch files using:

```bash
ros2 launch <package> <launch_file>
```

---

# 13. Hands-on: Launch TurtleBot3

## Step 1 — Set the Robot Model

For TurtleBot3 Burger:

```bash
export TURTLEBOT3_MODEL=burger
```

Check:

```bash
echo $TURTLEBOT3_MODEL
```

Expected:

```text
burger
```

---

# 14. Launch TurtleBot3 Simulation

Start the TurtleBot3 Gazebo simulation:

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Gazebo should open with the TurtleBot3 robot.

Keep this terminal running.

---

# 15. Explore ROS 2 Topics

Open a **new terminal**.

Source ROS 2 and the workspace:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

Set the robot model again if required:

```bash
export TURTLEBOT3_MODEL=burger
```

List active topics:

```bash
ros2 topic list
```

Important TurtleBot3 topics include:

```text
/cmd_vel
/odom
/scan
/imu
/joint_states
/tf
/tf_static
```

---

# 16. Inspect a Topic

Get information about `/cmd_vel`:

```bash
ros2 topic info /cmd_vel
```

Check the message type:

```bash
ros2 topic type /cmd_vel
```

Expected:

```text
geometry_msgs/msg/Twist
```

Display the message definition:

```bash
ros2 interface show geometry_msgs/msg/Twist
```

---

# 17. Teleoperate TurtleBot3

Open another terminal.

Source:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

Set the model:

```bash
export TURTLEBOT3_MODEL=burger
```

Start keyboard teleoperation:

```bash
ros2 run turtlebot3_teleop teleop_keyboard
```

Follow the instructions displayed in the terminal.

Use the keyboard to move the robot.

> Keep the teleoperation terminal active because it receives keyboard input.

---

# 18. Monitor `/cmd_vel`

Open another terminal:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

Run:

```bash
ros2 topic echo /cmd_vel
```

Now move the robot using teleoperation.

You should see velocity messages similar to:

```text
linear:
  x: ...
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: ...
```

The important values are:

```text
linear.x
```

and:

```text
angular.z
```

---

# 19. Monitor Odometry

Run:

```bash
ros2 topic echo /odom
```

The odometry message contains information about the robot's estimated motion.

You can also check the topic rate:

```bash
ros2 topic hz /odom
```

---

# 20. Launch RViz2

Start RViz2:

```bash
ros2 run rviz2 rviz2
```

RViz2 can be used to visualize:

* Robot model
* TF frames
* Laser scan
* Odometry
* Maps
* Navigation paths
* Costmaps

---

# 21. RViz2 Configuration

Set the **Fixed Frame** to:

```text
odom
```

Add the following displays.

### TF

Displays the robot coordinate frames.

### LaserScan

Set:

```text
Topic: /scan
```

### RobotModel

Displays the TurtleBot3 robot model.

### Odometry

Displays the robot's estimated movement.

---

# 22. Useful RViz2 Tools

Important RViz2 tools include:

### 2D Pose Estimate

Used with localization/Nav2 to tell the navigation system where the robot is located on the map.

### 2D Nav Goal

Used to send a navigation goal.

You can also change the camera view:

* Top-down
* Perspective
* Third-person
* First-person

---

# 23. ROS 2 System Introspection

List running nodes:

```bash
ros2 node list
```

Get information about a node:

```bash
ros2 node info /teleop_keyboard
```

List topics:

```bash
ros2 topic list
```

List topic types:

```bash
ros2 topic list -t
```

Check a topic:

```bash
ros2 topic info /scan
```

Check topic frequency:

```bash
ros2 topic hz /scan
```

---

# 24. Inspect LaserScan Messages

Display the LaserScan message definition:

```bash
ros2 interface show sensor_msgs/msg/LaserScan
```

Monitor the actual data:

```bash
ros2 topic echo /scan
```

This provides information such as:

* Range measurements
* Minimum range
* Maximum range
* Scan angle
* Scan increment

---

# 25. Inspect the TF Tree

TF describes relationships between coordinate frames.

You can inspect the TF system using:

```bash
ros2 run tf2_tools view_frames
```

Depending on the installed Jazzy version, this generates a TF frame report.

You can also inspect TF directly:

```bash
ros2 topic echo /tf
```

and:

```bash
ros2 topic echo /tf_static
```

---

# 26. View the ROS 2 Communication Graph

Run:

```bash
rqt_graph
```

If required:

```bash
rqt
```

The graph allows you to see relationships between:

```text
Nodes → Topics → Nodes
```

For example:

```text
teleop_keyboard
       |
       | /cmd_vel
       v
TurtleBot3 controller
       |
       v
Robot
```

---

# 27. Creating a TurtleBot3 Controller Package

Now we will create our own ROS 2 Python package.

Go to the workspace source directory:

```bash
cd ~/ros2_ws/src
```

Create the package:

```bash
ros2 pkg create turtlebot3_controller \
  --build-type ament_python \
  --dependencies rclpy geometry_msgs
```

The package will contain:

```text
turtlebot3_controller/
├── package.xml
├── resource/
├── setup.cfg
├── setup.py
└── turtlebot3_controller/
```

---

# 28. Create the Controller Script

Go into the Python package directory:

```bash
cd ~/ros2_ws/src/turtlebot3_controller/turtlebot3_controller
```

Create:

```bash
touch circle_controller.py
```

Open the file:

```bash
nano circle_controller.py
```

Add:

```python
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist


class Circle(Node):

    def __init__(self):
        super().__init__('turtlebot3_circle')

        self.vel_pub = self.create_publisher(
            Twist,
            '/cmd_vel',
            10
        )

        self.timer = self.create_timer(
            0.1,
            self.publish_velocity
        )

        self.get_logger().info(
            'TurtleBot3 circle controller started'
        )

    def publish_velocity(self):

        vel = Twist()

        # Linear velocity
        vel.linear.x = 0.2

        # No lateral or vertical movement
        vel.linear.y = 0.0
        vel.linear.z = 0.0

        # No rotation around X or Y
        vel.angular.x = 0.0
        vel.angular.y = 0.0

        # Angular velocity
        vel.angular.z = 0.5

        self.vel_pub.publish(vel)


def main(args=None):

    rclpy.init(args=args)

    node = Circle()

    try:
        rclpy.spin(node)

    except KeyboardInterrupt:
        pass

    finally:
        node.destroy_node()
        rclpy.shutdown()


if __name__ == '__main__':
    main()
```

---

# 29. Understanding the Circle Controller

The controller publishes:

```text
geometry_msgs/msg/Twist
```

to:

```text
/cmd_vel
```

The forward velocity is controlled using:

```python
vel.linear.x
```

The rotation is controlled using:

```python
vel.angular.z
```

For example:

```python
vel.linear.x = 0.2
vel.angular.z = 0.5
```

causes the robot to move forward while continuously rotating.

This produces a circular trajectory.

The approximate radius is:

```text
R = v / ω
```

where:

```text
v = linear velocity
ω = angular velocity
```

For:

```text
v = 0.2 m/s
ω = 0.5 rad/s
```

the approximate radius is:

```text
R = 0.2 / 0.5
R = 0.4 m
```

---

# 30. Add the Controller Entry Point

Open:

```bash
nano ~/ros2_ws/src/turtlebot3_controller/setup.py
```

Find:

```python
entry_points={
    'console_scripts': [
    ],
},
```

Change it to:

```python
entry_points={
    'console_scripts': [
        'circle_controller = turtlebot3_controller.circle_controller:main',
    ],
},
```

Save the file.

---

# 31. Build the Controller Package

Go to the workspace:

```bash
cd ~/ros2_ws
```

Build only the controller:

```bash
colcon build \
  --symlink-install \
  --packages-select turtlebot3_controller
```

Source the workspace:

```bash
source ~/ros2_ws/install/setup.bash
```

Check that ROS 2 can find it:

```bash
ros2 pkg list | grep turtlebot3_controller
```

---

# 32. Run the Circle Controller

First launch TurtleBot3:

```bash
export TURTLEBOT3_MODEL=burger
```

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Open another terminal and source:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

Run:

```bash
ros2 run turtlebot3_controller circle_controller
```

The TurtleBot3 should start moving in a circular path.

Stop the controller with:

```text
Ctrl+C
```

---

# 33. Important Safety Note

Before running a velocity controller, make sure the robot has sufficient space.

For simulation this means checking the Gazebo environment.

For a physical TurtleBot3:

* Keep the robot away from stairs
* Keep cables away from the wheels
* Keep people clear of the robot
* Always have a way to stop the robot
* Use low velocities during testing

---

# 34. Introduction to Nav2

Nav2 is the ROS 2 Navigation Stack.

It provides components for:

* Localization
* Mapping
* Global path planning
* Local trajectory planning
* Motion control
* Obstacle avoidance
* Recovery behaviors
* Navigation behavior trees

A simplified navigation pipeline is:

```text
Map
 │
 ▼
Localization
 │
 ▼
Global Planner
 │
 ▼
Controller
 │
 ▼
TurtleBot3
```

Sensor data such as LaserScan is used to detect obstacles.

---

# 35. Nav2 Architecture

Important Nav2 components include:

### BT Navigator

Coordinates navigation tasks using Behavior Trees.

### Planner Server

Computes a global path from the robot to the goal.

### Controller Server

Generates local velocity commands to follow the path.

### Behavior Server

Handles navigation behaviors and recovery actions.

### Smoother Server

Smooths planned paths.

### Lifecycle Manager

Manages the lifecycle state of Nav2 nodes.

### AMCL

Adaptive Monte Carlo Localization estimates the robot's position on a known map.

---

# 36. Launch the TurtleBot3 Simulation

Start Gazebo:

```bash
export TURTLEBOT3_MODEL=burger
```

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Keep Gazebo running.

---

# 37. Launch TurtleBot3 Navigation

Open a new terminal:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
```

Set the robot:

```bash
export TURTLEBOT3_MODEL=burger
```

Launch TurtleBot3 Navigation2:

```bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py \
  use_sim_time:=True \
  map:=$HOME/turtlebot3_ws/src/turtlebot3_simulations/turtlebot3_gazebo/maps/turtlebot3_world.yaml
```

> The exact map location depends on the TurtleBot3 repository version and workspace layout. If the above path does not exist, locate the map using:
>
> ```bash
> find ~/ros2_ws/src -name "turtlebot3_world.yaml"
> ```

Then use the returned path.

---

# 38. Simulation Time

Gazebo provides its own simulation clock.

Therefore Nav2 should use:

```text
use_sim_time:=True
```

For a real robot, simulation time is normally disabled:

```text
use_sim_time:=False
```

---

# 39. Check Nav2 Nodes

Run:

```bash
ros2 node list
```

You should see several Nav2-related nodes.

You can also check:

```bash
ros2 topic list
```

Important topics may include:

```text
/map
/scan
/odom
/cmd_vel
/tf
/tf_static
```

---

# 40. Open RViz2

Launch RViz2:

```bash
ros2 run rviz2 rviz2
```

Set:

```text
Fixed Frame: map
```

When Nav2 is running, RViz2 can display:

* Map
* Robot
* LaserScan
* TF
* Global path
* Local path
* Costmaps
* Navigation goal

---

# 41. Set the Initial Pose

The localization system needs an estimate of where the robot is on the map.

In RViz2:

1. Select **2D Pose Estimate**
2. Click the robot's approximate location on the map
3. Drag the arrow in the robot's heading direction
4. Release the mouse button

AMCL will use this information to initialize localization.

If localization does not converge:

* Try setting the pose again
* Make sure the map matches the simulation world
* Move the robot around using teleoperation
* Check `/scan`
* Check TF

---

# 42. Send a Navigation Goal

Once localization is working:

1. Select **2D Nav Goal**
2. Click the desired destination on the map
3. Drag the arrow to indicate the desired final orientation
4. Release

Nav2 will:

1. Receive the goal
2. Calculate a global path
3. Generate local motion commands
4. Avoid obstacles
5. Drive the robot toward the goal

---

# 43. Monitor Navigation

Use RViz2 to observe:

### Map

Shows the environment.

### Robot

Shows the estimated robot position.

### LaserScan

Shows sensor measurements.

### Global Path

Shows the planned route to the goal.

### Local Path

Shows the locally generated trajectory.

### Costmap

Shows obstacles and navigation costs.

### TF

Shows coordinate-frame relationships.

---

# 44. Useful ROS 2 Monitoring Commands

List nodes:

```bash
ros2 node list
```

List topics:

```bash
ros2 topic list
```

Monitor LaserScan:

```bash
ros2 topic echo /scan
```

Check LaserScan rate:

```bash
ros2 topic hz /scan
```

Monitor odometry:

```bash
ros2 topic echo /odom
```

Monitor velocity:

```bash
ros2 topic echo /cmd_vel
```

Inspect TF:

```bash
ros2 topic echo /tf
```

---

# 45. Useful Visualization Tools

Launch:

```bash
rqt
```

Or:

```bash
rqt_graph
```

`rqt_graph` helps visualize:

```text
Node → Topic → Node
```

This is useful for understanding how Nav2, sensors, controllers, and the robot communicate.

---

# 46. Troubleshooting

## TurtleBot3 package not found

If you see:

```text
Package 'turtlebot3_gazebo' not found
```

source ROS 2:

```bash
source /opt/ros/jazzy/setup.bash
```

then source the workspace:

```bash
source ~/ros2_ws/install/setup.bash
```

Check:

```bash
ros2 pkg list | grep turtlebot3
```

---

## TurtleBot3 model is not set

Set:

```bash
export TURTLEBOT3_MODEL=burger
```

Check:

```bash
echo $TURTLEBOT3_MODEL
```

---

## Gazebo does not start correctly

Check whether Gazebo-related packages are available:

```bash
ros2 pkg list | grep gazebo
```

Also check the TurtleBot3 Gazebo package:

```bash
ros2 pkg prefix turtlebot3_gazebo
```

---

## Controller package is not found

Check:

```bash
ros2 pkg list | grep turtlebot3_controller
```

If nothing appears:

```bash
cd ~/ros2_ws
colcon build --symlink-install --packages-select turtlebot3_controller
source ~/ros2_ws/install/setup.bash
```

---

## `/cmd_vel` is not available

Check:

```bash
ros2 topic list | grep cmd_vel
```

Then:

```bash
ros2 topic info /cmd_vel
```

Make sure TurtleBot3 simulation is running.

---

## Robot does not move

Check:

```bash
ros2 topic echo /cmd_vel
```

If velocity messages are being published but the robot does not move, inspect the simulation/controller side.

Also check:

```bash
ros2 node list
```

---

## Nav2 does not localize

Check:

```bash
ros2 topic echo /scan
```

Check TF:

```bash
ros2 topic echo /tf
```

Make sure:

```text
map → odom → base_link
```

is being published correctly.

Try setting the **2D Pose Estimate** again.

---

# 47. Complete Workshop Command Summary

## Terminal 1 — Gazebo

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash

export TURTLEBOT3_MODEL=burger

ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

---

## Terminal 2 — Teleoperation

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash

export TURTLEBOT3_MODEL=burger

ros2 run turtlebot3_teleop teleop_keyboard
```

---

## Terminal 3 — RViz2

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash

ros2 run rviz2 rviz2
```

---

## Terminal 4 — Topic Monitoring

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash

ros2 topic echo /cmd_vel
```

---

## Terminal 5 — Nav2

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash

export TURTLEBOT3_MODEL=burger

ros2 launch turtlebot3_navigation2 navigation2.launch.py \
  use_sim_time:=True \
  map:=<PATH_TO_TURTLEBOT3_WORLD_MAP>
```

---

# 48. Learning Objectives

After completing this workshop, you should be able to:

* Create a ROS 2 workspace
* Install and build TurtleBot3
* Understand ROS 2 packages
* Understand ROS 2 launch files
* Launch TurtleBot3 in simulation
* Identify ROS 2 nodes and topics
* Understand `/cmd_vel`, `/odom`, `/scan`, and TF
* Teleoperate TurtleBot3
* Visualize TurtleBot3 in RViz2
* Create a custom ROS 2 Python package
* Publish `geometry_msgs/msg/Twist`
* Control TurtleBot3 programmatically
* Understand the basics of Nav2
* Initialize AMCL localization
* Send navigation goals
* Monitor paths and costmaps
* Debug a TurtleBot3 navigation system

---

# 49. Final Workflow

The complete workflow is:

```text
ROS 2 Jazzy
     │
     ▼
ROS 2 Workspace
     │
     ▼
TurtleBot3 Packages
     │
     ▼
Gazebo Simulation
     │
     ├───────────────┐
     ▼               ▼
Teleoperation       RViz2
     │               │
     └───────┬───────┘
             ▼
        /cmd_vel
             │
             ▼
       TurtleBot3
             │
             ▼
           Nav2
             │
       ┌─────┴─────┐
       ▼           ▼
 Localization   Planning
       │           │
       └─────┬─────┘
             ▼
        Robot Control
             │
             ▼
       Navigation Goal
```

---

# 50. Conclusion

In this workshop, we started with a basic ROS 2 Jazzy workspace and installed TurtleBot3.

We then:

* Launched TurtleBot3 in Gazebo
* Explored ROS 2 topics
* Controlled the robot using teleoperation
* Visualized robot data in RViz2
* Inspected ROS 2 nodes and communication
* Created a custom TurtleBot3 controller
* Programmatically drove the robot in a circle
* Introduced the Nav2 architecture
* Localized the robot using AMCL
* Sent navigation goals through RViz2
* Monitored navigation and sensor data

This provides the foundation for more advanced TurtleBot3 work such as:

* SLAM
* Autonomous exploration
* Waypoint navigation
* Path planning
* Obstacle avoidance
* Custom Nav2 controllers
* Multi-robot navigation
* Robot perception

**End of Workshop**
