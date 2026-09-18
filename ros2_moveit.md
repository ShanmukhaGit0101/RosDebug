# Part 3 — MoveIt 2 Installation and Arm Workspace Setup

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
3. Navigate to the robot's Xacro file:

```text
ir52c/urdf/urdf.xml.xacro
```

4. Select `urdf.xml.xacro`.
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
