# Hands-on with TurtleBot3 — ROS 2 Humble

**ROS 2:** Humble  
**Ubuntu:** 22.04 LTS  
**Simulator:** Gazebo Classic 11  
**Robot:** TurtleBot3 Burger

This guide converts the supplied TurtleBot3 workshop material to ROS 2 Humble. It uses the Humble package names, TurtleBot3 Humble branches, Gazebo Classic, and `geometry_msgs/msg/Twist` for `/cmd_vel`.

(https://github.com/Sarup-A-K/ros_workshop/tree/main#hands-on-with-turtlebot3--humble--humble-conversion)

Replacements for each slide in `Hands_on_TurtleBot3.pdf`, targeting **ROS 2 Humble on Ubuntu 22.04 with Gazebo Classic 11**.

---

## ROS 2 Humble Setup

(https://github.com/Sarup-A-K/ros_workshop/tree/main#what-actually-changed)

| Area | ROS 2 Humble |
| ---- | ---- |
| Distro path | `/opt/ros/humble` |
| Git branch | `-b humble` |
| Simulator | Gazebo Classic 11 (`gazebo_ros`) |
| `/cmd_vel` type | `geometry_msgs/msg/Twist` |
| RViz config | TurtleBot3 Gazebo RViz configuration |
| TF tool | `view_frames.py` |
| `ros2 pkg create` | license optional |

The launch file names used in the workshop remain the same. The main Humble-specific points are the Gazebo Classic packages, `Twist` messages, TurtleBot3 RViz configuration, and the Humble TF command.

---

## Step 0 — Prepare Workspace & Dependencies

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-4--step-0-prepare-workspace--dependencies)

> Assume ROS 2 **Humble** + **Gazebo Classic 11** are already installed (Ubuntu 22.04).

```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

Source Humble:

```
source /opt/ros/humble/setup.bash
```

Install the simulation and navigation dependencies:

```
sudo apt update
sudo apt install -y \
  ros-humble-gazebo-ros-pkgs \
  ros-humble-navigation2 ros-humble-nav2-bringup \
  ros-humble-cartographer-ros \
  python3-colcon-common-extensions python3-rosdep
```

`ros-humble-gazebo-ros-pkgs` pulls in Gazebo Classic 11. Gazebo Classic is **not** available on Ubuntu 22.04 — it reached end of life in January 2025, which is why the simulator stack changed.

---

## Step 1 — Clone TurtleBot3 Repositories

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-5--step-1-clone-turtlebot3-repositories)

### Clone Core Packages

(https://github.com/Sarup-A-K/ros_workshop/tree/main#clone-core-packages)

```
cd ~/ros2_ws/src
# Core TurtleBot3 packages (Humble branch)
git clone -b humble \
  https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone -b humble \
  https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
```

### Clone Simulation Package

(https://github.com/Sarup-A-K/ros_workshop/tree/main#clone-simulation-package)

```
# Gazebo Classic simulation for TurtleBot3
git clone -b humble \
  https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
```

### Optional — real hardware only

(https://github.com/Sarup-A-K/ros_workshop/tree/main#optional--real-hardware-only)

```
git clone -b humble \
  https://github.com/ROBOTIS-GIT/DynamixelSDK.git
```

**Result:** `turtlebot3`, `turtlebot3_msgs`, and `turtlebot3_simulations` are now in `~/ros2_ws/src`.

> **Shortcut for a lab full of machines:** Humble binaries exist, so
> `sudo apt install ros-humble-turtlebot3 ros-humble-turtlebot3-msgs ros-humble-turtlebot3-gazebo`
> skips the clone-and-build entirely. Worth checking with `apt search ros-humble-turtlebot3`
> before the session — it saves students a 10-minute build.

---

## Step 2 — Build & Source the Workspace

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-6--step-2-build--source-the-workspace)

### Resolve dependencies first

(https://github.com/Sarup-A-K/ros_workshop/tree/main#resolve-dependencies-first)

```
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
```

### Build with colcon

(https://github.com/Sarup-A-K/ros_workshop/tree/main#build-with-colcon)

```
cd ~/ros2_ws
colcon build --symlink-install
```

### Source the Overlay

(https://github.com/Sarup-A-K/ros_workshop/tree/main#source-the-overlay)

```
source ~/ros2_ws/install/setup.bash
```

- Repeat the `source` command in every new terminal (or add it to `~/.bashrc`).
- Set the model once in `~/.bashrc` so students stop forgetting it: 
  ```
  echo 'export TURTLEBOT3_MODEL=burger' >> ~/.bashrc
  ```

---

## ROS 2 Launch Files

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slides-813--ros-2-launch-files)

**No major changes are needed.** `LaunchDescription`, `launch_ros.actions.Node`, `generate_launch_description()`, and the `setup.py` `data_files` glob work in Humble.

For Humble, the launch-file installation is the important part. A license field is recommended but is not required for the basic workshop.

```
# Add to imports:
from glob import glob
import os

# Add to data_files in setup():
(os.path.join('share', package_name, 'launch'), glob('launch/*.py')),

# And in setup():
license='Apache-2.0',
```

---

## Step 3 — Launch Simulation

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-15--step-3-launch-simulation)

### Start Gazebo Classic Simulation Environment

(https://github.com/Sarup-A-K/ros_workshop/tree/main#start-gazebo-harmonic-simulation-environment)

```
# Choose the robot model
export TURTLEBOT3_MODEL=burger

# Launch TurtleBot3 World in Gazebo Classic
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

The simulation uses **Gazebo Classic 11** with the `gazebo_ros` integration.

Other worlds:

```
ros2 launch turtlebot3_gazebo empty_world.launch.py
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```

---

## Step 4 — Explore Available ROS 2 Topics

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-16--step-4-explore-available-ros-2-topics)

```
ros2 topic list
```

Expected topics include:

- `/cmd_vel` — Velocity commands (input) → **`geometry_msgs/msg/Twist`**
- `/odom` — Odometry data (output)
- `/scan` — Laser scan data
- `/imu` — Inertial measurement unit data
- `/joint_states` — Robot joint states
- `/tf` and `/tf_static` — Transform data

Useful live demo:

```
ros2 topic type /cmd_vel
ros2 interface show geometry_msgs/msg/Twist
```

---

## Step 5 — Teleoperate the Robot

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-17--step-5-teleoperate-the-robot)

```
ros2 run turtlebot3_teleop teleop_keyboard
```

The teleoperation command publishes `geometry_msgs/msg/Twist` to `/cmd_vel`. Keep the teleoperation terminal active so it receives keyboard input.

---

## Step 6 — Monitor Topic Activity

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-18--step-6-monitor-topic-activity)

```
ros2 topic echo /cmd_vel
ros2 topic echo /odom
```

```
rqt_graph
rqt
```

The output of `ros2 topic echo /cmd_vel` contains the `linear` and `angular` fields directly because Humble uses `geometry_msgs/msg/Twist`.

---

## Step 7 — Launch RViz2

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-19--step-7-launch-rviz2)

For Humble, you can launch RViz2 directly and load the TurtleBot3 configuration manually, or use the TurtleBot3 Gazebo RViz configuration if it is installed.

```
# Launch RViz2
ros2 run rviz2 rviz2
```

Key displays to add (unchanged): **TF**, **LaserScan** (`/scan`), **RobotModel**, **Odometry**, **PointCloud2**.

---

## RViz2 Configuration Tips

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-20--rviz2-configuration-tips)

Use the RViz2 navigation tools available in your installed Nav2/RViz configuration. In the Humble TurtleBot3 workflow, the navigation goal tool is commonly shown as **2D Nav Goal**.

- Global Options → Fixed Frame: `odom`
- LaserScan → Topic `/scan`, Size 0.1 m
- Interactive Markers → **2D Pose Estimate**, **2D Nav Goal**

---

## Step 8 — Additional Exploration Commands

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-21--step-8-additional-exploration-commands)

```
# View all running nodes
ros2 node list
# View node information
ros2 node info /teleop_keyboard
# View topic message structure
ros2 interface show sensor_msgs/msg/LaserScan
# Monitor specific topic rate
ros2 topic hz /scan
# View TF tree
ros2 run tf2_tools view_frames.py
```

---

## Step 1 — Create a ROS 2 Package

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-23--step-1-create-a-ros-2-package)

```
cd ~/ros2_ws/src
ros2 pkg create turtlebot3_controller \
  --build-type ament_python \
  --dependencies rclpy geometry_msgs
```

The package is created as an `ament_python` package with `rclpy` and `geometry_msgs` dependencies.

---

## Step 2 — Controller Script

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-24--step-2-controller-script-rewritten-for-humble)

Create `turtlebot3_controller/circle_controller.py`.

Three things changed from the Humble version:

1. `Twist` → `Twist` (velocity now lives under `msg.twist`, plus a `header`).
2. Publishing once inside `__init__` never worked reliably — the publisher has not finished discovery yet, so the single message is dropped. Use a timer.
3. `linear.x = 1.0` exceeds the burger's maximum of 0.22 m/s; it gets clamped and the circle is not the radius you advertise.

```
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class Circle(Node):
    def __init__(self):
        super().__init__('turtlebot3_controller')
        self.get_logger().info('Node started with turtlebot3_circle')

        self.vel_pub = self.create_publisher(Twist, '/cmd_vel', 10)

        self.linear_x = 0.15    # m/s
        self.angular_z = 0.5    # rad/s

        radius = self.linear_x / self.angular_z
        self.get_logger().info(f'Radius = {radius:.2f} m')

        self.timer = self.create_timer(0.1, self.publish_velocity)  # 10 Hz
```

---

## Slide 25 — Step 2 continued

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-25--step-2-continued)

```
    def publish_velocity(self):
        msg = Twist()
        msg.linear.x = self.linear_x
        msg.angular.z = self.angular_z
        self.vel_pub.publish(msg)

    def stop(self):
        msg = Twist()
        self.vel_pub.publish(msg)

def main(args=None):
    rclpy.init(args=args)
    node = Circle()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass
    finally:
        node.stop()
        node.destroy_node()
        if rclpy.ok():
            rclpy.shutdown()

if __name__ == '__main__':
    main()
```

- **Publisher:** publishes `Twist` velocity commands to `/cmd_vel`.
- **Velocity setup:** `linear.x` controls forward speed, `angular.z` the rotation rate.
- **Radius:** r = v / ω, so 0.15 / 0.5 = 0.3 m.
- Stopping on `Ctrl+C` matters — otherwise the robot keeps circling after the node dies.

---

## `setup.py` Entry Point

(https://github.com/Sarup-A-K/ros_workshop/tree/main#new-slide-worth-adding--setuppy-entry-point)

Add the console entry point so `ros2 run` can find the controller:

```
entry_points={
    'console_scripts': [
        'circle_controller = turtlebot3_controller.circle_controller:main',
    ],
},
```

---

## Step 3 — Build the Package

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-26--step-3-build-the-package)

```
cd ~/ros2_ws
colcon build --symlink-install --packages-select turtlebot3_controller
```

```
source ~/ros2_ws/install/setup.bash
```

Unchanged.

---

## Step 4 — Launch Simulation and Run Controller

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-27--step-4-launch-simulation-and-run-controller)

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

```
ros2 run turtlebot3_controller circle_controller
```

**Result:** The TurtleBot3 follows a circular path of radius ≈ 0.3 m in Gazebo Classic.

---

## Nav2 Step 1 — Navigation Setup

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-31--nav2-step-1-navigation-setup)

### Simulation Environment

(https://github.com/Sarup-A-K/ros_workshop/tree/main#simulation-environment)

```
# Launch Gazebo Classic simulation
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### Real Robot Environment

(https://github.com/Sarup-A-K/ros_workshop/tree/main#real-robot-environment)

```
# Launch navigation with pre-built map
ros2 launch nav2_bringup bringup_launch.py \
  use_sim_time:=false \
  map:=$HOME/tb3_world_map.yaml
```

- **Simulation:** `use_sim_time:=True` for the Gazebo clock
- **Real robot:** `use_sim_time:=false` for the system clock
- **Map path:** ensure the path to your saved map file is correct

> You need a map before this step. Build one with:
>
> ```
> ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
> ros2 run nav2_map_server map_saver_cli -f ~/map
> ```

---

## Nav2 Step 2 — Launch Navigation Stack

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-32--nav2-step-2-launch-navigation-stack)

```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_navigation2 navigation2.launch.py \
  use_sim_time:=True \
  map:=$HOME/map.yaml
```

Components launched (unchanged): AMCL localization, global + local planners, DWB controller, behavior trees.

**Humble note:** the TurtleBot3/Nav2 setup in this workshop uses `geometry_msgs/msg/Twist` on `/cmd_vel`, matching the controller and teleoperation examples.

---

## Set Initial Pose and Navigation Goal

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slides-3334--set-initial-pose-and-navigation-goal)

Substantively unchanged. Only the toolbar label for the goal tool differs:

- **2D Pose Estimate** — same name, same behaviour
- **2D Goal Pose** — this was "2D Nav Goal" in the Humble-era RViz2

Troubleshooting notes on both slides still apply.

---

## Monitor Robot Motion

(https://github.com/Sarup-A-K/ros_workshop/tree/main#slide-35--monitor-robot-motion)

Unchanged. `rqt` tools (Topic Monitor, Node&#x20;
Graph, Bag Recording, Plotting, Console, Robot Steering) and the RViz2&#x20;
displays (Map, Path, LaserScan, TF, Costmaps) all work the same.

For Humble, Robot Steering and other tools that publish plain `geometry_msgs/msg/Twist` are compatible with the `/cmd_vel` convention used in this workshop.

## About

No description, website, or topics provided.

### Resources

(https://github.com/Sarup-A-K/ros_workshop/tree/main#readme-ov-file)

(https://github.com/Sarup-A-K/ros_workshop/activity)

### Stars

svg**0** stars

### Watchers

svg**0** watching

### Forks

(https://github.com/Sarup-A-K/ros_workshop/forks)[**0**](https://github.com/Sarup-A-K/ros_workshop/forks)[ forks](https://github.com/Sarup-A-K/ros_workshop/forks)

[Report repository](https://github.com/contact/report-content?content_url=https%3A%2F%2Fgithub.com%2FSarup-A-K%2Fros_workshop\&report=Sarup-A-K+%28user%29)
