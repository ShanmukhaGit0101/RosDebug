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
3. Navigate to the robot Xacro file:

```text
~/arm_workspace/src/ros_workshop/IR52C_description/urdf/arm.urdf.xacro
```

4. Select `arm.urdf.xacro`.
5. Click **Load Files**.

## 4. Generate the Self-Collision Matrix

Open the **Self-Collisions** section.

1. Click **Generate Collision Matrix**.
2. Wait for the collision matrix to be generated.

## 5. Add a Virtual Joint

Open the **Virtual Joints** section and click **Add Virtual Joint**.

| Setting | Value |
|---|---|
| Virtual Joint Name | `virtual_joint` |
| Child Link | `base_link` |
| Parent Frame | `world` |
| Joint Type | `fixed` |

Save the virtual joint.

## 6. Create a Planning Group

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
2. Confirm the generated MoveIt controller configuration.

## 10. Select the Configuration Package Location

In the **Configuration Files** section, 

Set the configuration package location to:

```text
~/arm_workspace/src/ros_workshop/moveit_config
```

The MoveIt Setup Assistant will create the `moveit_config` package at this location.

Click **Generate Package** to create the MoveIt configuration.

> **Note:** Do not select the `arm_workspace` root. The generated package should be placed inside `arm_workspace/src/ros_workshop`.

## 11. Close MoveIt Setup Assistant

After the configuration package has been generated successfully, close the MoveIt Setup Assistant.

## 12. Go to the Arm Workspace

Open a terminal and go to the arm workspace.

```bash
cd ~/arm_workspace
```

## 13. Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

## 14. Build the Workspace

Build the workspace again to include the newly generated MoveIt package.

```bash
colcon build --symlink-install
```

## 15. Source the Workspace

Source the newly built workspace.

```bash
source ~/arm_workspace/install/setup.bash
```

## 16. Launch the MoveIt Demo

Launch the generated MoveIt configuration.

```bash
ros2 launch moveit_config demo.launch.py
```
