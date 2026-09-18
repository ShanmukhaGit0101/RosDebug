# Part 3 — MoveIt 2 Installation and Arm Workspace Setup

### 1. Source ROS 2 Jazzy

Load the ROS 2 Jazzy environment.

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

Create a separate workspace for the arm and its MoveIt packages.

```bash
mkdir -p ~/arm_workspace/src
```

### 5. Go to the Workspace

```bash
cd ~/arm_workspace
```

### 6. Build the Workspace

Build the empty workspace using `colcon`.

```bash
colcon build --symlink-install
```

### 7. Source the Workspace

After the build completes, source the workspace.

```bash
source ~/arm_workspace/install/setup.bash
```
