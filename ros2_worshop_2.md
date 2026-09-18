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

The simulation requires separate terminals for the different ROS 2 processes.

## Terminal 1 — Start Gazebo

Open a new terminal and start Gazebo:

```bash
gazebo
```

Leave this terminal running.

---

## Terminal 2 — Launch the AMR Simulation

Open another terminal.

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

## Terminal 3 — Start the AMR Handler

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

# Terminal Summary

At this stage, you should have the following terminals running:

| Terminal       | Command                                     | Purpose                   |
| -------------- | ------------------------------------------- | ------------------------- |
| **Terminal 1** | `gazebo`                                    | Starts Gazebo             |
| **Terminal 2** | `ros2 launch amr_gazebo gazebo.launch.py`   | Starts the AMR simulation |
| **Terminal 3** | `ros2 launch amr_handler handler.launch.py` | Starts the AMR handler    |

For every new ROS 2 terminal, remember to source both:

```bash
source /opt/ros/jazzy/setup.bash
source ~/amr_workshop/install/setup.bash
```

The workspace setup is now complete and the AMR simulation can be started.
