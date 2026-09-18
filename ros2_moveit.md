# Part 1 — ROS 2 Workshop Setup

This section prepares the ROS 2 workspace, downloads the workshop repository, builds the packages, and installs keyboard teleoperation.

> **Prerequisite:** ROS 2 Jazzy should already be installed on the system.

---

## 1. Create the Workshop Workspace

Create the `arm_workshop` workspace and its `src` directory.

```bash
mkdir -p ~/arm_workshop/src
```

## 2. Enter the `src` Directory

ROS 2 packages are stored inside the `src` directory of the workspace.

```bash
cd ~/arm_workshop/src
```

## 3. Clone the Workshop Repository

Clone the workshop repository from GitHub.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

The repository will be located at:

```text
~/arm_workshop/src/ros_workshop
```

## 4. Remove the Existing `workshop_demo` Package

We will create our own `workshop_demo` package later in the workshop.

```bash
rm -rf ~/arm_workshop/src/ros_workshop/workshop_demo
```

## 5. Go to the Workspace Root

Return to the workspace root before building.

```bash
cd ~/arm_workshop
```

> **Important:** Run `colcon build` from the workspace root.

## 6. Source ROS 2 Jazzy

Load the ROS 2 Jazzy environment.

```bash
source /opt/ros/jazzy/setup.bash
```

## 7. Build the Workspace

Build the workshop packages using `colcon`.

```bash
colcon build --symlink-install
```

## 8. Source the Workshop Workspace

Source the workspace after the build completes.

```bash
source ~/arm_workshop/install/setup.bash
```

## 9. Install Keyboard Teleoperation

Update the package lists.

```bash
sudo apt update
```

Install the ROS 2 keyboard teleoperation package.

```bash
sudo apt install ros-jazzy-teleop-twist-keyboard
```

---

# Part 2 — Start the arm Simulation

## Terminal 1 — Launch the arm Simulation

Open a new terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/arm_workshop/install/setup.bash
```

### Launch the arm Simulation

```bash
ros2 launch arm_gazebo gazebo.launch.py
```

Leave this terminal running.

## Terminal 2 — Start the arm Handler

Open another terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/arm_workshop/install/setup.bash
```

### Launch the arm Handler

```bash
ros2 launch arm_handler handler.launch.py
```

Leave this terminal running.

## Terminal 3 — Start Keyboard Teleoperation

Open another terminal.

### Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### Source the Workshop Workspace

```bash
source ~/arm_workshop/install/setup.bash
```

### Start Teleoperation

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

Use the keys shown in the terminal to control the arm.

## Terminal Summary

| Terminal | Command | Purpose |
|---|---|---|
| **Terminal 1** | `ros2 launch arm_gazebo gazebo.launch.py` | Launches the arm simulation |
| **Terminal 2** | `ros2 launch arm_handler handler.launch.py` | Starts the arm handler |
| **Terminal 3** | `ros2 run teleop_twist_keyboard teleop_twist_keyboard` | Controls the arm |

For every new ROS 2 terminal, source both:

```bash
source /opt/ros/jazzy/setup.bash
```

```bash
source ~/arm_workshop/install/setup.bash
```

---

# Part 3 — MoveIt 2 Installation and Arm Workspace Setup

This section installs MoveIt 2 and creates a separate workspace for the robot arm.

## 1. Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

## 2. Update Package Lists

```bash
sudo apt update
```

## 3. Install MoveIt 2

Install MoveIt 2 for ROS 2 Jazzy.

```bash
sudo apt install ros-jazzy-moveit
```

## 4. Create the Arm Workspace

Create the arm workspace and its `src` directory.

```bash
mkdir -p ~/arm_workspace/src
```

## 5. Enter the `src` Directory

```bash
cd ~/arm_workspace/src
```

## 6. Clone the Workshop Repository

Clone the workshop repository into the arm workspace.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

## 7. Go to the Workspace Root

```bash
cd ~/arm_workspace
```

## 8. Build the Arm Workspace

Build the packages using `colcon`.

```bash
colcon build --symlink-install
```

## 9. Source the Arm Workspace

Load the newly built packages into the current terminal.

```bash
source ~/arm_workspace/install/setup.bash
```

---

# Part 4 — Create the MoveIt Configuration

## 1. Set the Qt Platform

Set the Qt platform to `xcb` before launching the MoveIt Setup Assistant.

```bash
export QT_QPA_PLATFORM=xcb
```

## 2. Open MoveIt Setup Assistant

Launch the MoveIt Setup Assistant.

```bash
ros2 launch moveit_setup_assistant setup_assistant.launch.py
```

## 3. Load the Robot Xacro

In the MoveIt Setup Assistant GUI:

1. Select **Create New MoveIt Configuration Package**.
2. Click **Browse**.
3. Navigate to:

```text
ir52c/urdf/urdf.xml.xacro
```

4. Select `urdf.xml.xacro`.
5. Click **Load Files**.

## 4. Generate the Self-Collision Matrix

Open the **Self-Collisions** section.

Click **Generate Collision Matrix** and wait for the matrix to be generated.

## 5. Add a Virtual Joint

Open the **Virtual Joints** section and click **Add Virtual Joint**.

| Setting | Value |
|---|---|
| Virtual Joint Name | `virtual_joint` |
| Child Link | `base_link` |
| Parent Frame | `world` |
| Joint Type | `fixed` |

Save the virtual joint.

## 6. Create the Planning Group

Open **Planning Groups** and click **Add Group**.

| Setting | Value |
|---|---|
| Group Name | `arm` |
| Kinematic Solver | `KDL Kinematics Plugin` |

Under **Add Kinematic Chain**, add the kinematic chain for the arm.

Then click **Save Group**.

## 7. Create Robot Poses

Open **Robot Poses**.

Create two poses:

- `home`
- `ready`

For each pose, use the joint sliders to set a configuration and save the pose.

## 8. Configure ROS 2 Controllers

Open the **ROS 2 Controllers** section.

1. Click **Auto Add**.
2. Add the **Joint Trajectory Controller**.
3. Confirm the generated controller configuration.

## 9. Configure MoveIt Controllers

Open the **MoveIt Controllers** section.

1. Click **Auto Add**.
2. Confirm the generated MoveIt controller configuration.# Part 3 — MoveIt 2 Installation and Arm Workspace Setup

### 1. Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

### 2. Update package lists

```bash
sudo apt update
```

### 3. Install MoveIt 2

Install MoveIt 2 for ROS 2 Jazzy.

```bash
sudo apt install ros-jazzy-moveit
```

### 4. Create the Arm Workspace

Create the workspace and its `src` directory.

```bash
mkdir -p ~/arm_workspace/src
```

### 5. Go to the `src` Directory

```bash
cd ~/arm_workspace/src
```

### 6. Clone the Workshop Repository

Clone the workshop repository into the arm workspace.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

### 7. Go to the Workspace Root

```bash
cd ~/arm_workspace
```

### 8. Build the Workspace

Build the packages using `colcon`.

```bash
colcon build --symlink-install
```

### 9. Source the Workspace

Load the newly built packages into the current terminal.

```bash
source ~/arm_workspace/install/setup.bash
```
### 10. Open MoveIt Setup Assistant

Launch the MoveIt Setup Assistant to create the MoveIt configuration for the robot.

```bash
ros2 launch moveit_setup_assistant setup_assistant.launch.py
```

### 10. Set the Qt Platform

Set the Qt platform to `xcb` before launching the MoveIt Setup Assistant.

```bash
source ~/arm_workspace/install/setup.bash
source /opt/ros/jazzy/setup.bash
export QT_QPA_PLATFORM=xcb
```

### 11. Open MoveIt Setup Assistant

Launch the MoveIt Setup Assistant.

```bash
ros2 launch moveit_setup_assistant setup_assistant.launch.py
```

### 12. Load the Robot Xacro

In the MoveIt Setup Assistant GUI:

1. Select **Create New MoveIt Configuration Package**.
2. Click **Browse**.
3. Navigate to the robot Xacro file:

```text
arm_workshop/src/ros_workshop/IR52C_description/urdf/arm.urdf.xacro
```

4. Select `arm.urdf.xacro`.
5. Click **Load Files**.

### 13. Generate the Self-Collision Matrix

In the **Self-Collisions** section:

1. Click **Generate Collision Matrix**.
2. Wait for the collision matrix to be generated.

### 14. Add a Virtual Joint

Go to the **Virtual Joints** section and click **Add Virtual Joint**.

Set the following values:

| Setting            | Value           |
| ------------------ | --------------- |
| Virtual Joint Name | `virtual_joint` |
| Child Link         | `base_link`     |
| Parent Frame       | `world`         |
| Joint Type         | `fixed`         |

Save the virtual joint.

### 15. Create a Planning Group

Go to **Planning Groups** and click **Add Group**.

Set:

* **Group Name:** `arm`
* **Kinematic Solver:** `KDL Kinematics Plugin`

Under **Add Kinematic Chain**, add the kinematic chain for the arm.

Then click **Save Group**.

### 16. Create Robot Poses

Go to **Robot Poses**.

Click **Add Pose** and use the sliders to move the robot to a random configuration.

Create two poses:

* `home`
* `ready`

Use the sliders to set the joint positions for each pose, then save each pose.

### 17. Configure ROS 2 Controllers

Go to the **ROS 2 Controllers** section.

1. Click **Auto Add** to automatically add the controller.
2. Select the **Joint Trajectory Controller**.
3. Confirm the generated controller configuration.

### 18. Configure MoveIt Controllers

Go to the **MoveIt Controllers** section.

1. Click **Auto Add**.
2. This automatically adds the MoveIt controller configuration for the joint trajectory controller.
3. Confirm the generated configuration.

## 4. Select the Configuration Package Location

In the **Configuration Files** section, select a location inside your home directory.

Create the configuration folder with the name:

```text
moveit_config_
```

> **Note:** Do **not** select your current `arm_workspace` location. The MoveIt Setup Assistant generates MoveIt packages in the selected location, and using a workspace that already contains ROS 2 packages can cause package conflicts or errors.
