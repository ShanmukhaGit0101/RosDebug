# Part 1 — ROS 2 Workshop Setup

This section prepares the ROS 2 workspace, downloads the workshop repository, builds the packages, and starts the AMR simulation.

> **Prerequisite:** ROS 2 Jazzy should already be installed on the system.

---

## 1. Create the Workshop Workspace

Create the `amr_workshop` workspace and its `src` directory.

```bash
mkdir -p ~/amr_workshop/src
```

---

## 2. Enter the `src` Directory

ROS 2 packages are stored inside the `src` directory of the workspace.

```bash
cd ~/amr_workshop/src
```

---

## 3. Clone the Workshop Repository

Clone the workshop repository from GitHub.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

After cloning, the repository will be located at:

```text
~/amr_workshop/src/ros_workshop
```

---

## 4. Remove the Existing `workshop_demo` Package

The repository contains an existing `workshop_demo` package. We will create our own version later in the workshop, so remove the existing copy.

```bash
rm -rf ~/amr_workshop/src/ros_workshop/workshop_demo
```

---

## 5. Go to the Workspace Root

Return to the workspace root before building.

```bash
cd ~/amr_workshop
```

> **Important:** `colcon build` should normally be run from the workspace root, not from the `src` directory.

---

## 6. Source ROS 2 Jazzy

Load the ROS 2 Jazzy environment into the current terminal.

```bash
source /opt/ros/jazzy/setup.bash
```

---

## 7. Build the Workspace

Build all packages in the workshop workspace using `colcon`.

```bash
colcon build --symlink-install
```

The `--symlink-install` option is useful during development because changes to Python files can be reflected without reinstalling the files each time.

---

## 8. Source the Workshop Workspace

After the build completes successfully, source the workspace.

```bash
source ~/amr_workshop/install/setup.bash
```

This makes the packages built inside `amr_workshop` available to the current terminal.

---

## 9. Install Keyboard Teleoperation

Install the ROS 2 keyboard teleoperation package.

First update the package lists:

```bash
sudo apt update
```

Then install the package:

```bash
sudo apt install ros-jazzy-teleop-twist-keyboard
```

---

# Part 2 — Start the AMR Simulation

## Terminal 1 — Launch the AMR Simulation

Open a terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/amr_workshop/install/setup.bash
```

### Launch the AMR Simulation

```bash
ros2 launch amr_gazebo gazebo.launch.py
```

Leave this terminal running.

---

## Terminal 2 — Start the AMR Handler

Open another terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/amr_workshop/install/setup.bash
```

### Launch the AMR Handler

```bash
ros2 launch amr_handler handler.launch.py
```

Leave this terminal running.

---

## Terminal 3 — Start Keyboard Teleoperation

Open another terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/amr_workshop/install/setup.bash
```

### Start Teleoperation

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Use the keys shown in the terminal to control the AMR.

---

# Terminal Summary

| Terminal       | Command                                                | Purpose                             |
| -------------- | ------------------------------------------------------ | ----------------------------------- |
| **Terminal 1** | `ros2 launch amr_gazebo gazebo.launch.py`              | Launches the AMR simulation         |
| **Terminal 2** | `ros2 launch amr_handler handler.launch.py`            | Starts the AMR handler              |
| **Terminal 3** | `ros2 run teleop_twist_keyboard teleop_twist_keyboard` | Controls the AMR using the keyboard |

For every new ROS 2 terminal, source both:

```bash
source /opt/ros/jazzy/setup.bash
```

```bash
source ~/amr_workshop/install/setup.bash
```

# Part 3 — ROS 2 System Inspection

## 1. List Active ROS 2 Nodes

Display all currently running ROS 2 nodes.

```bash
ros2 node list
```

## 2. List Available ROS 2 Topics

Display all currently available ROS 2 topics.

```bash
ros2 topic list
```

## 3. View TF Frame Tree

Generate the TF frame tree of the robot.

```bash
ros2 run tf2_tools view_frames
```


Experiment: Circular Navigation of the AMR
Task

Create a new ROS 2 Python node for circular navigation.

The new node must:

Publish velocity commands using the Twist message.
Publish those commands to the /cmd_vel topic.
Make the AMR move continuously in a circular path.
Work together with the existing diff_drive_node.
Allow the circular-navigation node to run in a separate terminal, independently of the existing teleoperation setup.

The existing diff_drive_node already receives Twist messages from /cmd_vel and converts the linear and angular velocities into wheel velocities.

Guidance
1. Identify where the new node belongs

The package structure currently contains:

ros_workshop/
└── amr_handler/
    ├── amr_handler/
    │   ├── __init__.py
    │   ├── diff_drive_node.py
    │   └── odometry_node.py
    ├── launch/
    ├── resource/
    ├── package.xml
    └── setup.py

Create the new navigation Python file inside the amr_handler/amr_handler/ directory alongside the existing Python nodes.

The new file should be named:

circular_nav_node.py
2. Design the circular navigation node

The node should:

Create a ROS 2 node.
Create a publisher for Twist.
Publish to /cmd_vel.
Publish repeatedly at a suitable rate.
Set both:
forward linear velocity
angular velocity
Keep publishing while the node is running.

Do not modify the existing diff_drive_node.py for this task.

3. Understand how circular motion is produced

Think about the two components of a Twist command:

Linear velocity controls how fast the robot moves forward.
Angular velocity controls how fast the robot turns.

For circular motion, both need to be present.

The exact values should be selected so that the robot produces a stable circular trajectory.

As a challenge, participants should experiment with the relationship between linear and angular velocity to change the radius of the circle.

4. Register the new node

Creating the Python file alone is not enough.

The new node must be registered as an executable in the package's setup.py.

Participants should locate the existing entry_points section and add an entry for the new circular_nav_node.

Do not remove or modify the existing diff_drive_node and odometry_node entries.

5. Check dependencies

The new node uses:

rclpy
geometry_msgs

Participants should verify that the package already declares the required dependencies.

If a required dependency is missing, add it to the appropriate section of package.xml.

6. Build the package

After creating and registering the new node, rebuild the amr_handler package.

The purpose of rebuilding is to make ROS 2 aware of the newly added executable.

After building, source the updated workspace before testing.

7. Run the experiment in parallel

The experiment should use separate terminals.

Terminal 1 — Existing AMR system

Run the normal AMR simulation/handler and teleoperation setup.

The existing launch file starts both the diff_drive_node and odometry_node.

Terminal 2 — Circular navigation

Run the newly created circular_nav_node.

The new node should publish Twist commands to /cmd_vel.

The communication should therefore be:

circular_nav_node
       │
       │ Twist
       ▼
   /cmd_vel
       │
       ▼
diff_drive_node
       │
       ▼
 left/right wheels
       │
       ▼
      AMR
8. Verify the experiment

Participants should verify:

The new node is running.
/cmd_vel exists.
circular_nav_node is publishing on /cmd_vel.
diff_drive_node is receiving the commands.
The robot moves continuously.
The robot follows a circular trajectory.
Odometry continues to be generated.
