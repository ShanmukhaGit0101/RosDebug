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

# Part 4 — Circular Navigation Using a Custom Twist Publisher

## Task

Create a **custom ROS 2 Python node** that makes the AMR move in a **circular path** by publishing `Twist` messages to the `/cmd_vel` topic.

The new node should work with the existing `diff_drive_node` without modifying the existing drive or odometry nodes.

---

## 1. Understand the Requirement

The custom node should:

- Be written in Python.
- Create a ROS 2 node.
- Publish `geometry_msgs/msg/Twist` messages.
- Publish the messages to `/cmd_vel`.
- Continuously provide both linear and angular velocity.
- Generate a circular motion for the AMR.

Before creating any files, inspect the existing **AMR workspace architecture** and understand how the `amr_handler` package is organized.

---

## 2. Generate the Python Script Using Generative AI

Use a Generative AI tool to generate a ROS 2 Python script for circular navigation.

Your prompt should clearly specify that the node must:

- Publish `Twist` messages.
- Use the `/cmd_vel` topic.
- Continuously publish velocity commands.
- Use both linear and angular velocity.
- Make a differential-drive AMR follow a circular path.
- Be suitable for integration into the existing `amr_handler` package.

Review the generated script and make sure you understand the purpose of the node, publisher, topic, message type, and velocity commands.

> **Do not directly modify the existing `diff_drive_node.py` or `odometry_node.py`.**

---

## 3. Create the `scripts` Folder

Inside the `amr_handler` Python package, create a folder named `scripts`.

Create the new Python file inside this folder.

The resulting structure should be similar to:

```text
amr_workshop/
├── build/
├── install/
├── log/
└── src/
    └── ros_workshop/
        ├── amr_description/
            |---
        ├── amr_gazebo/
            |---
        ├── amr_handler/
        │   ├── amr_handler/
        │   │   ├── __init__.py
        │   │   ├── diff_drive_node.py
        │   │   ├── odometry_node.py
        │   │   └── scripts/
        │   │       └── circular_nav_node.py
        │   │
        │   ├── launch/
        │   │   └── handler.launch.py
        │   ├── resource/
        │   │   └── amr_handler
        │   ├── package.xml
        │   ├── setup.py
        │   └── setup.cfg

```

> **Note:** The exact package structure may differ. Always check your existing `amr_handler` package before creating the new folder and file.

Paste the generated circular-navigation script into `circular_nav_node.py`.

---

## 4. Register the New Node

Before registering the node, inspect the package and identify how Python executables are configured.

### If `setup.py` exists

Add the new `circular_nav_node` as an executable under the existing `entry_points` section.

Keep the existing:

- `diff_drive_node`
- `odometry_node`

unchanged.

### If `setup.py` does not exist

Do **not** create one blindly.

Check the existing package build configuration and follow the method already used by that package to register Python executables.

The new node must be registered so that ROS 2 can discover and run it.

---

## 5. Check Dependencies

Make sure the package has the required dependencies for the new node.

The circular navigation node requires:

```text
rclpy
geometry_msgs
```

Check the existing `package.xml` and add any missing dependencies using the same structure already used by the package.

---

## 6. Build the Workspace

After creating the new node and registering it:

1. Build the `amr_handler` package.
2. Check that the build completes successfully.
3. Source the updated workspace.
4. Verify that the new `circular_nav_node` executable can be discovered.

---

# Part 5 — Initial Testing

## Three-Terminal Test

Before running the final experiment, test the existing AMR system using three terminals.

### Terminal 1 — Gazebo

Launch the AMR simulation.

### Terminal 2 — AMR Handler

Launch the AMR handler.

### Terminal 3 — Teleoperation

Start keyboard teleoperation and verify that the AMR can be controlled normally.

After confirming that the system works correctly, stop all three terminals.

---

# Part 6 — Final Circular Navigation Experiment

Start the complete system using **four terminals**.

Run them in the following sequence:

### Terminal 1 — Gazebo

Launch the AMR simulation.

### Terminal 2 — AMR Handler

Launch the AMR handler.

### Terminal 3 — Teleoperation

Start keyboard teleoperation.

### Terminal 4 — Circular Navigation

Run the newly created `circular_nav_node`.

The communication flow should be:

```text
circular_nav_node
       |
       | Twist
       v
   /cmd_vel
       |
       v
diff_drive_node
       |
       v
  AMR wheel motion
       |
       v
 Circular Path
```

---

## Expected Result

The custom navigation node should continuously publish `Twist` commands through `/cmd_vel`.

The existing `diff_drive_node` should receive these commands and control the AMR's wheel motion.

The AMR should move continuously in a **circular path** while the Gazebo simulation, AMR handler, teleoperation, and circular navigation node are running.


