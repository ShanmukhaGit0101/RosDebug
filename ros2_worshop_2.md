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



