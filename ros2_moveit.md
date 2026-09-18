# Part 3 — Install MoveIt 2

### 1. Source ROS 2 Jazzy

Load the ROS 2 Jazzy environment.

```bash
source /opt/ros/jazzy/setup.bash
```

### 2. Update package lists

Update the Ubuntu package lists before installing MoveIt 2.

```bash
sudo apt update
```

### 3. Install MoveIt 2

Install the MoveIt 2 packages for ROS 2 Jazzy.

```bash
sudo apt install ros-jazzy-moveit
```

### 4. Verify the installation

Check that MoveIt packages are available.

```bash
ros2 pkg list | grep moveit
```

### 5. Open MoveIt Setup Assistant

The MoveIt Setup Assistant is used to create a MoveIt configuration package for a robot.

```bash
ros2 launch moveit_setup_assistant setup_assistant.launch.py
```
