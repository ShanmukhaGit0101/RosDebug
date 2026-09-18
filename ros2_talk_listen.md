# ROS 2 Jazzy Beginner Workshop: Workspace, Python Package, Talker & Listener

# Quick Start — Run These Commands First

> **Beginner rule:** Copy the commands below in order.  
> The explanations for **why** each command is needed are given later in this document.

## 1. Source ROS 2 Jazzy

```bash
source /opt/ros/jazzy/setup.bash
```

Check:

```bash
echo $ROS_DISTRO
```

Expected:

```text
jazzy
```

---

## 2. Create the workspace

```bash
mkdir -p ~/ros_ws/src
cd ~/ros_ws
```

---

## 3. Create the Python ROS 2 package

```bash
cd ~/ros_ws/src
ros2 pkg create --build-type ament_python workshop_demo 
```

---

## 4. Clone the workshop repository

```bash
cd ~
git clone https://github.com/anyarobotics/ros_workshop.git
```

---

## 5. Copy the talker and listener

```bash
cp ~/ros_workshop/workshop_demo/workshop_demo/talker.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/

cp ~/ros_workshop/workshop_demo/workshop_demo/listener.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/
```

Check:

```bash
ls ~/ros_ws/src/workshop_demo/workshop_demo
```

You should see:

```text
__init__.py
listener.py
talker.py
```

---

## 6. Add the executables to `setup.py`

Open:

```bash
nano ~/ros_ws/src/workshop_demo/setup.py
```

Make sure the `entry_points` section contains:

```python
entry_points={
    'console_scripts': [
        'talker = workshop_demo.talker:main',
        'listener = workshop_demo.listener:main',
    ],
},
```

Save with:

```text
Ctrl + O
Enter
```

Exit with:

```text
Ctrl + X
```

---

## 7. Build the workspace

> **Important:** Run `colcon build` from `~/ros_ws`, **not** from `~/ros_ws/src`.

```bash
cd ~/ros_ws
colcon build --symlink-install
```

---

## 8. Source the workspace

```bash
source ~/ros_ws/install/setup.bash
```

Verify:

```bash
ros2 pkg list | grep workshop_demo
```

Expected:

```text
workshop_demo
```

---

#  Run the Talker and Listener

## Terminal 1 — Talker

Open a new terminal and run:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo talker
```

You should see messages similar to:

```text
Publishing: [5, 7]
```

**Leave this terminal running.**

---

## Terminal 2 — Listener

Open another terminal and run:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo listener
```

You should see:

```text
Received: 5, 7 | Sum = 12
```

---

# Optional Test Commands

Open another terminal:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
```

### List nodes

```bash
ros2 node list
```

Expected:

```text
/listener
/talker
```

### List topics

```bash
ros2 topic list
```

Look for:

```text
/numbers
```

### See the messages

```bash
ros2 topic echo /numbers
```

### Check the topic type

```bash
ros2 topic type /numbers
```

Expected:

```text
std_msgs/msg/Int32MultiArray
```

### See topic information

```bash
ros2 topic info /numbers
```

---

# 🖥️ Install RViz2 and Gazebo

Run these after the talker/listener exercise:

```bash
sudo apt update
```

Install ROS development tools:

```bash
sudo apt install \
    ros-dev-tools \
    python3-colcon-common-extensions
```

Install RViz2 and common robot tools:

```bash
sudo apt install \
    ros-jazzy-rviz2 \
    ros-jazzy-xacro \
    ros-jazzy-robot-state-publisher \
    ros-jazzy-joint-state-publisher \
    ros-jazzy-joint-state-publisher-gui \
    ros-jazzy-tf2-tools
```

Install ROS 2 ↔ Gazebo integration:

```bash
sudo apt install ros-jazzy-ros-gz
```

If needed, install Gazebo Harmonic:

```bash
sudo apt install gz-harmonic
```

Test RViz2:

```bash
source /opt/ros/jazzy/setup.bash
rviz2
```

Test Gazebo:

```bash
gz sim
```

---

# 📌 If You Only Want the Essential Commands

For the workshop itself, the core workflow is:

```bash
source /opt/ros/jazzy/setup.bash

mkdir -p ~/ros_ws/src
cd ~/ros_ws/src

ros2 pkg create --build-type ament_python workshop_demo --dependencies rclpy std_msgs

cd ~
git clone https://github.com/anyarobotics/ros_workshop.git

cp ~/ros_workshop/workshop_demo/workshop_demo/talker.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/

cp ~/ros_workshop/workshop_demo/workshop_demo/listener.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/

# Edit setup.py and add talker/listener entry points

cd ~/ros_ws
colcon build --symlink-install

source install/setup.bash
```

Then:

**Terminal 1**

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo talker
```

**Terminal 2**

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo listener
```

---

# 1. What We Are Going to Build

The final directory structure will look approximately like this:

```text
~/ros_ws/
├── src/
│   └── workshop_demo/
│       ├── package.xml
│       ├── setup.py
│       ├── setup.cfg
│       ├── resource/
│       │   └── workshop_demo
│       ├── test/
│       └── workshop_demo/
│           ├── __init__.py
│           ├── talker.py
│           └── listener.py
│
├── build/
├── install/
└── log/
```

There are three important levels here:

```text
ros_ws/
└── src/
    └── workshop_demo/       <-- ROS 2 package
        └── workshop_demo/   <-- Python module
            ├── talker.py
            └── listener.py
```


The basic ROS 2 development cycle is therefore:

```text
CREATE / EDIT
      ↓
BUILD
      ↓
SOURCE
      ↓
RUN
      ↓
TEST
      ↓
EDIT AGAIN
      ↓
BUILD AGAIN
```

