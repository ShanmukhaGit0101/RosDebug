# Part 1 — ROS 2 Workshop Setup

### 1. Create the workspace

Create the `amr_workshop` workspace and its `src` folder.

```bash
mkdir -p ~/amr_workshop/src
```

### 2. Go to the `src` folder

ROS 2 packages will be created inside the `src` folder.

```bash
cd ~/amr_workshop/src
```

### 3. Clone the workshop repository

Clone the workshop files from GitHub.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

### 4. Remove the existing `workshop_demo`

We will create our own `workshop_demo` package later, so remove the copy from the cloned repository.

```bash
rm -rf ~/amr_workshop/src/ros_workshop/workshop_demo
```

### 5. Go to the workspace root

`colcon build` should be run from the workspace root, not from the `src` folder.

```bash
cd ~/amr_workshop
```

### 6. Source ROS 2 Jazzy

This loads the ROS 2 Jazzy environment into the current terminal.

```bash
source /opt/ros/jazzy/setup.bash
```

### 7. Build the workspace

`colcon` is the ROS 2 build tool. Run it from `~/amr_workshop`.

```bash
colcon build --symlink-install
```

### 8. Source the workspace

This makes the packages built inside `amr_workshop` available to the current terminal.

```bash
source ~/amr_workshop/install/setup.bash
```

### 9. Install Teleop Keyboard

Install the ROS 2 keyboard teleoperation package.

```bash
sudo apt update
```

```bash
sudo apt install ros-jazzy-teleop-twist-keyboard
```

### 10. Launch Gazebo

Start Gazebo.

```bash
gazebo
```

### 11. Source ROS 2 Jazzy

Open a new terminal and source ROS 2 Jazzy.

```bash
source /opt/ros/jazzy/setup.bash
```

### 12. Source the workshop workspace

Load the packages built in the workshop workspace.

```bash
source ~/amr_workshop/install/setup.bash
```

### 13. Launch the AMR simulation

Launch the robot simulation in Gazebo.

```bash
ros2 launch amr_gazebo gazebo.launch.py
```

### 14. Start the AMR handler

Open another terminal and source ROS 2 Jazzy.

```bash
source /opt/ros/jazzy/setup.bash
```

### 15. Source the workshop workspace

```bash
source ~/amr_workshop/install/setup.bash
```

### 16. Launch the handler

Start the AMR handler.

```bash
ros2 launch amr_handler handler.launch.py
```
# Part 1 — ROS 2 Workshop Setup

### 1. Create the workspace

Create the `amr_workshop` workspace and its `src` folder.

```bash
mkdir -p ~/amr_workshop/src
```

### 2. Go to the `src` folder

ROS 2 packages will be created inside the `src` folder.

```bash
cd ~/amr_workshop/src
```

### 3. Clone the workshop repository

Clone the workshop files from GitHub.

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

### 4. Remove the existing `workshop_demo`

We will create our own `workshop_demo` package later, so remove the copy from the cloned repository.

```bash
rm -rf ~/amr_workshop/src/ros_workshop/workshop_demo
```

### 5. Go to the workspace root

`colcon build` should be run from the workspace root, not from the `src` folder.

```bash
cd ~/amr_workshop
```

### 6. Source ROS 2 Jazzy

This loads the ROS 2 Jazzy environment into the current terminal.

```bash
source /opt/ros/jazzy/setup.bash
```

### 7. Build the workspace

`colcon` is the ROS 2 build tool. Run it from `~/amr_workshop`.

```bash
colcon build --symlink-install
```

### 8. Source the workspace

This makes the packages built inside `amr_workshop` available to the current terminal.

```bash
source ~/amr_workshop/install/setup.bash
```

### Install Teleop Keyboard

Install the ROS 2 keyboard teleoperation package.

```bash
sudo apt update
sudo apt install ros-jazzy-teleop-twist-keyboard
