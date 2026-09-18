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
