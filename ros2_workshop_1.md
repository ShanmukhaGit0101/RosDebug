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

This double `workshop_demo` is normal for an `ament_python` package.

---

# 2. First Understand Three Important Words

Before typing commands, understand these three terms.

## 2.1 Workspace

A **workspace** is simply a folder where you keep your ROS 2 packages and the files generated when those packages are built.

We will call ours:

```text
ros_ws
```

So:

```text
~/ros_ws
```

means:

> A folder named `ros_ws` inside your home directory.

---

## 2.2 Package

A **package** is a ROS 2 software unit.

Our package will be:

```text
workshop_demo
```

It will contain our Python ROS 2 nodes.

---

## 2.3 Node

A **node** is a running ROS 2 program.

In this workshop:

- `talker.py` becomes the **talker node**
- `listener.py` becomes the **listener node**

The talker publishes data.

The listener receives the data.

The repository's current Python files publish and subscribe to the `numbers` topic using `std_msgs/msg/Int32MultiArray`. The talker publishes `[5, 7]`, and the listener receives the two numbers and prints their sum. citeturn3view1turn3view2

---

# 3. Before Starting: Source ROS 2 Jazzy

Open a terminal.

Run:

```bash
source /opt/ros/jazzy/setup.bash
```

Now check:

```bash
echo $ROS_DISTRO
```

You should get:

```text
jazzy
```

You can also check:

```bash
ros2 --help
```

If `ros2` is recognized, ROS 2 is available in this terminal.

ROS 2's documentation explicitly requires the ROS installation setup file to be sourced before using the ROS environment. citeturn1search0

---

# 4. What Does `source /opt/ros/jazzy/setup.bash` Mean?

This is one of the most important concepts for a beginner.

When ROS 2 is installed, it is installed under something similar to:

```text
/opt/ros/jazzy/
```

Inside it are:

- ROS 2 commands
- ROS 2 Python libraries
- ROS 2 message definitions
- ROS 2 package information
- environment configuration

But simply having those files on the disk does not automatically tell your current terminal where everything is.

The command:

```bash
source /opt/ros/jazzy/setup.bash
```

loads the ROS 2 environment into the **current terminal**.

Think of it like:

```text
ROS 2 installed on disk
        |
        | source
        v
Current terminal now knows about ROS 2
```

That is why, in a fresh terminal, you normally source Jazzy before running ROS commands.

---

# 5. Optional: Automatically Source Jazzy in Every Terminal

If you do not want to manually type this every time, you can add it to your Bash configuration.

Run:

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```

Then reload Bash:

```bash
source ~/.bashrc
```

Check:

```bash
echo $ROS_DISTRO
```

Expected:

```text
jazzy
```

After this, newly opened terminals should automatically know about ROS 2 Jazzy.

> If you have multiple ROS distributions installed, be careful with this step. Automatically sourcing one distribution can cause confusion if you later work with another distribution.

---

# 6. Create the ROS 2 Workspace

We will now create:

```text
~/ros_ws
```

The `src` folder is where ROS 2 source packages belong.

Run:

```bash
mkdir -p ~/ros_ws/src
```

Now enter the workspace:

```bash
cd ~/ros_ws
```

Check your location:

```bash
pwd
```

You should see something similar to:

```text
/home/YOUR_USERNAME/ros_ws
```

Now check:

```bash
ls
```

You should see:

```text
src
```

---

# 7. Where Should `colcon build` Be Run?

This is extremely important for beginners.

### Run `colcon build` from the workspace root.

For our workspace:

```text
~/ros_ws
```

So:

```bash
cd ~/ros_ws
colcon build
```

### Do NOT normally run it from:

```text
~/ros_ws/src
```

and do not run it from:

```text
~/ros_ws/src/workshop_demo
```

The normal structure is:

```text
~/ros_ws
    |
    +-- src/
    |
    +-- build/
    |
    +-- install/
    |
    +-- log/
```

Therefore `colcon` is normally run here:

```text
~/ros_ws
```

ROS 2 documentation and ROS training material similarly build from the directory above `src`. citeturn1search2turn1search3

---

# 8. What Is `colcon`?

`colcon` is the build tool commonly used with ROS 2.

You can think of it as:

> "Take the ROS 2 packages in my workspace, process/build them, and install them into the workspace so ROS 2 can use them."

For example:

```bash
cd ~/ros_ws
colcon build
```

will look inside:

```text
~/ros_ws/src
```

and find ROS packages.

It then creates:

```text
~/ros_ws/build
~/ros_ws/install
~/ros_ws/log
```

---

# 9. What Are `build`, `install`, and `log`?

After:

```bash
colcon build
```

you will normally have:

```text
ros_ws/
├── src/
├── build/
├── install/
└── log/
```

## `src/`

Contains your source code.

For example:

```text
src/workshop_demo/workshop_demo/talker.py
```

This is where you edit your code.

---

## `build/`

Contains intermediate build information generated by `colcon`.

You normally do not edit this manually.

---

## `install/`

This is especially important.

It contains the installed version of your ROS packages and their environment information.

When you run:

```bash
source install/setup.bash
```

you are telling the terminal:

> "Also make the packages I built in this workspace available to ROS 2."

---

## `log/`

Contains build and test logs.

If a build fails, `log/` can help diagnose what happened.

---

# 10. Why Do We Need Both `source` Commands?

This is one of the most important ideas in ROS 2.

There are two different environments:

### System ROS 2

```bash
source /opt/ros/jazzy/setup.bash
```

This gives you:

```text
ROS 2 Jazzy itself
```

### Your workspace

```bash
source ~/ros_ws/install/setup.bash
```

This gives you:

```text
Packages that YOU built in ros_ws
```

Think of it like this:

```text
/opt/ros/jazzy/setup.bash
          |
          v
      ROS 2 Jazzy
          |
          v
    Your workspace
          |
          v
~/ros_ws/install/setup.bash
          |
          v
   workshop_demo
```

You generally need the base ROS environment first, and then your workspace overlay.

The ROS documentation demonstrates this pattern: source the ROS installation, build the workspace, and then source the workspace's `install/setup.bash` so its packages become discoverable. citeturn1search2turn4search6

---

# 11. Create the Python Package

First make sure you are using Jazzy:

```bash
source /opt/ros/jazzy/setup.bash
```

Go to the source directory:

```bash
cd ~/ros_ws/src
```

Now create an `ament_python` package:

```bash
ros2 pkg create --build-type ament_python workshop_demo --dependencies rclpy std_msgs
```

This creates a Python ROS 2 package named:

```text
workshop_demo
```

The `--build-type ament_python` part tells ROS 2:

> "This package uses Python and the ROS 2 ament Python build system."

The `--dependencies` part declares that our package needs:

```text
rclpy
std_msgs
```

The official Jazzy documentation uses the same `ros2 pkg create --build-type ament_python` approach for Python packages. citeturn1search2

---

# 12. Check the Package

Run:

```bash
cd ~/ros_ws/src
ls
```

You should see:

```text
workshop_demo
```

Now:

```bash
cd workshop_demo
ls
```

You should see files/folders similar to:

```text
package.xml
resource
setup.cfg
setup.py
test
workshop_demo
```

Now:

```bash
cd workshop_demo
ls
```

You should see:

```text
__init__.py
```

The important distinction is:

```text
~/ros_ws/src/workshop_demo
```

is the ROS package directory.

Inside it:

```text
~/ros_ws/src/workshop_demo/workshop_demo
```

is the Python module directory.

---

# 13. Clone the Workshop Repository

The workshop repository is:

```text
https://github.com/anyarobotics/ros_workshop.git
```

The repository contains a `workshop_demo` Python package, including:

```text
workshop_demo/talker.py
workshop_demo/listener.py
```

Its `setup.py` registers these executables as:

```text
talker
listener
```

under the `workshop_demo` package. citeturn1view0turn3view0

We do **not** need to clone the entire repository directly into our `src` folder because we have already created our own package.

Instead, clone it temporarily and copy the two Python files we need.

---

# 14. Clone the Repository Temporarily

Go to your home directory:

```bash
cd ~
```

Clone:

```bash
git clone https://github.com/anyarobotics/ros_workshop.git
```

Now check:

```bash
ls
```

You should see:

```text
ros_workshop
```

Check its contents:

```bash
cd ~/ros_workshop
ls
```

You should find:

```text
workshop_demo
```

The repository currently contains a `workshop_demo` directory. citeturn1view0

---

# 15. Copy `talker.py` and `listener.py`

Our destination is:

```text
~/ros_ws/src/workshop_demo/workshop_demo/
```

Run:

```bash
cp ~/ros_workshop/workshop_demo/workshop_demo/talker.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/
```

Then:

```bash
cp ~/ros_workshop/workshop_demo/workshop_demo/listener.py \
   ~/ros_ws/src/workshop_demo/workshop_demo/
```

Check:

```bash
ls ~/ros_ws/src/workshop_demo/workshop_demo
```

You should now see:

```text
__init__.py
listener.py
talker.py
```

---

# 16. What Do These Two Programs Do?

## Talker

The repository's `talker.py`:

1. Creates a ROS 2 node named `talker`
2. Creates a publisher
3. Publishes `Int32MultiArray`
4. Publishes on the topic:

```text
numbers
```

5. Publishes:

```text
[5, 7]
```

every second. citeturn3view1

Conceptually:

```text
talker
   |
   | publishes [5, 7]
   v
/numbers
```

---

## Listener

The `listener.py`:

1. Creates a ROS 2 node named `listener`
2. Subscribes to:

```text
numbers
```

3. Receives:

```text
[5, 7]
```

4. Adds them:

```text
5 + 7 = 12
```

5. Prints the result. citeturn3view2

Conceptually:

```text
talker
   |
   | [5, 7]
   v
numbers topic
   |
   v
listener
   |
   v
Sum = 12
```

---

# 17. Configure `setup.py`

This step is important.

The Python files existing in the folder does **not** automatically mean that:

```bash
ros2 run workshop_demo talker
```

will work.

ROS 2 needs to know which Python functions should become executable ROS commands.

Open:

```bash
nano ~/ros_ws/src/workshop_demo/setup.py
```

Find:

```python
entry_points={
    'console_scripts': [
    ],
},
```

Change it to:

```python
entry_points={
    'console_scripts': [
        'talker = workshop_demo.talker:main',
        'listener = workshop_demo.listener:main',
    ],
},
```

Save:

```text
Ctrl + O
Enter
```

Exit:

```text
Ctrl + X
```

The original workshop repository uses these same two console-script entries. citeturn3view0

---

# 18. Why Do We Need `entry_points`?

This:

```python
'talker = workshop_demo.talker:main'
```

means:

```text
ROS executable name
        |
        v
     talker
        |
        v
Python module
        |
        v
workshop_demo.talker
        |
        v
Python function
        |
        v
main()
```

Similarly:

```python
'listener = workshop_demo.listener:main'
```

means:

```text
ros2 run workshop_demo listener
```

will eventually call:

```python
workshop_demo/listener.py
```

and:

```python
main()
```

---

# 19. Check `package.xml`

Open:

```bash
nano ~/ros_ws/src/workshop_demo/package.xml
```

Because we created the package using:

```bash
--dependencies rclpy std_msgs
```

the package should already contain the dependencies.

You should have entries similar to:

```xml
<depend>rclpy</depend>
<depend>std_msgs</depend>
```

Do not delete them.

The two Python programs import:

```python
import rclpy
```

and:

```python
from std_msgs.msg import Int32MultiArray
```

so these dependencies are required. citeturn3view1turn3view2

---

# 20. Build the Workspace

Now comes the most important build step.

Go to the **workspace root**:

```bash
cd ~/ros_ws
```

Make sure you are NOT inside:

```text
~/ros_ws/src
```

You should be here:

```text
~/ros_ws
```

Check:

```bash
pwd
```

Expected:

```text
/home/YOUR_USERNAME/ros_ws
```

Now build:

```bash
colcon build --symlink-install
```

---

# 21. What Does `--symlink-install` Mean?

Normally, `colcon` installs/copies Python files into the install area.

With:

```bash
colcon build --symlink-install
```

the development workspace uses symbolic links where appropriate.

This is convenient during development because changing a Python source file can be reflected without having to copy the source file again in the same way as a normal install.

ROS 2 documentation specifically describes `--symlink-install` as useful for development because source files can be linked into the build/install environment. citeturn4search6

For a beginner workshop, I recommend:

```bash
colcon build --symlink-install
```

---

# 22. What Should a Successful Build Look Like?

At the end, you should see something similar to:

```text
Summary: 1 package finished
```

or a similar successful summary.

Now check:

```bash
ls
```

You should have:

```text
build
install
log
src
```

---

# 23. Source Your Workspace

After building, run:

```bash
source ~/ros_ws/install/setup.bash
```

Or, because you are already inside the workspace:

```bash
source install/setup.bash
```

This is the second important `source` command.

Remember:

```bash
source /opt/ros/jazzy/setup.bash
```

means:

> "Give this terminal the ROS 2 Jazzy environment."

Whereas:

```bash
source ~/ros_ws/install/setup.bash
```

means:

> "Add my newly built ROS 2 workspace packages to this terminal's ROS environment."

---

# 24. Verify That ROS Can Find Your Package

Run:

```bash
ros2 pkg list | grep workshop_demo
```

Expected:

```text
workshop_demo
```

You can also run:

```bash
ros2 pkg prefix workshop_demo
```

You should get a path containing:

```text
~/ros_ws/install/workshop_demo
```

This confirms that the package has been built and installed into the workspace.

---

# 25. Check the Available Executables

Run:

```bash
ros2 run workshop_demo
```

Depending on the ROS CLI behavior/version, it may show available executables or usage information.

You can also check the installed package using:

```bash
ros2 pkg executables workshop_demo
```

Expected output should include:

```text
workshop_demo listener
workshop_demo talker
```

---

# 26. Run the Talker — Terminal 1

Open **Terminal 1**.

First source ROS 2:

```bash
source /opt/ros/jazzy/setup.bash
```

Then source your workspace:

```bash
source ~/ros_ws/install/setup.bash
```

Now run:

```bash
ros2 run workshop_demo talker
```

You should see messages similar to:

```text
[INFO] [talker]: Publishing: [5, 7]
```

The talker will continue publishing once per second.

Leave this terminal running.

---

# 27. Run the Listener — Terminal 2

Open a **second terminal**.

Again:

```bash
source /opt/ros/jazzy/setup.bash
```

Then:

```bash
source ~/ros_ws/install/setup.bash
```

Now run:

```bash
ros2 run workshop_demo listener
```

You should see something similar to:

```text
[INFO] [listener]: Received: 5, 7 | Sum = 12
```

The listener should continue receiving the numbers.

---

# 28. Important Correction About `ros2 run`

The command is **not**:

```bash
ros2 run listener
```

and it is not:

```bash
ros2 run talker
```

The normal ROS 2 syntax is:

```text
ros2 run <PACKAGE_NAME> <EXECUTABLE_NAME>
```

Therefore ours is:

```bash
ros2 run workshop_demo talker
```

and:

```bash
ros2 run workshop_demo listener
```

Think:

```text
ros2 run
   |
   +-- package = workshop_demo
   |
   +-- executable = talker/listener
```

---

# 29. Complete Terminal 1 Command Sequence

For the talker:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo talker
```

---

# 30. Complete Terminal 2 Command Sequence

For the listener:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo listener
```

---

# 31. What Is Actually Happening?

The complete communication chain is:

```text
                 ROS 2 DDS communication
                         |
                         v
+----------------+    /numbers    +----------------+
|     TALKER     |  ------------> |    LISTENER    |
|                |                |                |
| publishes      |                | subscribes     |
| [5, 7]         |                | receives [5,7] |
+----------------+                +----------------+
                                         |
                                         v
                                     5 + 7
                                         |
                                         v
                                       12
```

The two programs do not directly call each other.

They communicate through the ROS 2 topic:

```text
/numbers
```

---

# 32. Inspect the Topic

While the talker and listener are running, open another terminal.

Source ROS:

```bash
source /opt/ros/jazzy/setup.bash
```

Source the workspace:

```bash
source ~/ros_ws/install/setup.bash
```

List topics:

```bash
ros2 topic list
```

You should see:

```text
/numbers
```

---

# 33. See the Messages Directly

Run:

```bash
ros2 topic echo /numbers
```

You should see something similar to:

```text
data:
- 5
- 7
---
data:
- 5
- 7
---
```

This lets you observe the data without running your listener node.

---

# 34. Check the Topic Type

Run:

```bash
ros2 topic type /numbers
```

Expected:

```text
std_msgs/msg/Int32MultiArray
```

This matches the message type used by the workshop's `talker.py` and `listener.py`. citeturn3view1turn3view2

---

# 35. See Who Publishes and Subscribes

Run:

```bash
ros2 topic info /numbers
```

You should see information indicating:

```text
Publisher count: 1
Subscription count: 1
```

This is a useful way to understand ROS 2 communication.

---

# 36. See Running Nodes

Run:

```bash
ros2 node list
```

You should see:

```text
/listener
/talker
```

This confirms that both ROS 2 nodes are running.

---

# 37. The Most Important Beginner Workflow

Whenever you create or modify a ROS 2 package, remember this basic workflow:

```text
1. Source ROS 2
       |
       v
2. Edit/create package
       |
       v
3. Go to workspace root
       |
       v
4. colcon build
       |
       v
5. Source install/setup.bash
       |
       v
6. ros2 run / ros2 launch
```

In commands:

```bash
source /opt/ros/jazzy/setup.bash

cd ~/ros_ws

colcon build --symlink-install

source install/setup.bash

ros2 run workshop_demo talker
```

---

# 38. What If I Modify `talker.py`?

Suppose you edit:

```text
~/ros_ws/src/workshop_demo/workshop_demo/talker.py
```

For a safe beginner workflow, do:

```bash
cd ~/ros_ws
colcon build --symlink-install
source install/setup.bash
```

Then run it again:

```bash
ros2 run workshop_demo talker
```

With `--symlink-install`, Python development is more convenient because the installed development environment can link back to source files. citeturn4search6

---

# 39. Common Beginner Mistake: Building in `src`

### Wrong:

```bash
cd ~/ros_ws/src
colcon build
```

### Correct:

```bash
cd ~/ros_ws
colcon build
```

Remember:

```text
             WORKSPACE ROOT
                  |
                  v
             ~/ros_ws
                  |
          +-------+-------+
          |               |
         src            build
                          |
                       install
                          |
                         log
```

`src` contains source packages.

The workspace root is where `colcon build` is normally run.

---

# 40. Common Beginner Mistake: Forgetting to Source Jazzy

If you open a new terminal and run:

```bash
ros2 run workshop_demo talker
```

you may get an error such as:

```text
ros2: command not found
```

or ROS may not know the required environment.

Run:

```bash
source /opt/ros/jazzy/setup.bash
```

Then:

```bash
source ~/ros_ws/install/setup.bash
```

Then:

```bash
ros2 run workshop_demo talker
```

---

# 41. Common Beginner Mistake: Forgetting to Source the Workspace

Suppose you built successfully:

```bash
cd ~/ros_ws
colcon build
```

but then open a new terminal.

This does not automatically mean the new terminal knows about `workshop_demo`.

Run:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
```

Then:

```bash
ros2 run workshop_demo talker
```

---

# 42. Common Beginner Mistake: Running Before Building

This will normally not work if your package has never been built:

```bash
ros2 run workshop_demo talker
```

Build first:

```bash
cd ~/ros_ws
colcon build --symlink-install
```

Then:

```bash
source install/setup.bash
```

Then run:

```bash
ros2 run workshop_demo talker
```

---

# 43. Common Beginner Mistake: Wrong Python File Location

The files should be here:

```text
~/ros_ws/src/workshop_demo/workshop_demo/talker.py
~/ros_ws/src/workshop_demo/workshop_demo/listener.py
```

Not here:

```text
~/ros_ws/src/talker.py
```

and not here:

```text
~/ros_ws/talker.py
```

The Python module is:

```text
workshop_demo/
    workshop_demo/
        talker.py
        listener.py
```

---

# 44. Optional: Remove the Temporary Git Clone

Once the files have been copied successfully, you no longer need the temporary clone.

You can remove it:

```bash
rm -rf ~/ros_workshop
```

**Do not run this command if you still want to keep the cloned repository.**

Your actual ROS workspace remains:

```text
~/ros_ws
```

---

# 45. Install Development Tools

If `colcon` is missing, install the ROS development tools:

```bash
sudo apt update
sudo apt install ros-dev-tools
```

The official Jazzy installation documentation lists `ros-dev-tools` as the development-tools package. citeturn1search0

You can also install the common colcon extensions:

```bash
sudo apt install python3-colcon-common-extensions
```

---

# 46. Install RViz2

If you installed:

```bash
sudo apt install ros-jazzy-desktop
```

RViz2 is included as part of the ROS 2 desktop installation.

If ROS 2 is already installed but you specifically want the RViz2 package, you can install:

```bash
sudo apt update
sudo apt install ros-jazzy-rviz2
```

Then source Jazzy:

```bash
source /opt/ros/jazzy/setup.bash
```

Run:

```bash
rviz2
```

You should see the RViz2 graphical interface.

RViz2 is the visualization tool you will commonly use to visualize:

- Robot models
- TF frames
- Laser scans
- Point clouds
- Camera data
- Maps
- Navigation information
- Robot states

---

# 47. Install Useful RViz/Robot Packages

For beginner robotics work, these are useful:

```bash
sudo apt update

sudo apt install \
    ros-jazzy-rviz2 \
    ros-jazzy-xacro \
    ros-jazzy-robot-state-publisher \
    ros-jazzy-joint-state-publisher \
    ros-jazzy-joint-state-publisher-gui \
    ros-jazzy-tf2-tools
```

These are useful when moving from simple nodes to robot models and URDFs.

---

# 48. Install Gazebo for ROS 2 Jazzy

For ROS 2 Jazzy, use the modern **Gazebo** stack rather than the old Gazebo Classic workflow.

Jazzy is associated with **Gazebo Harmonic**.

Install the ROS-Gazebo integration:

```bash
sudo apt update
sudo apt install ros-jazzy-ros-gz
```

The `ros_gz` packages provide the ROS 2 ↔ Gazebo integration.

Gazebo documentation describes ROS 2 Jazzy and later workflows using `ros_gz_sim`, including commands such as `ros2 launch ros_gz_sim ...`. citeturn4search7

---

# 49. Install Gazebo and Common ROS Simulation Dependencies

For a general beginner robotics environment, install:

```bash
sudo apt update

sudo apt install \
    ros-jazzy-ros-gz \
    ros-jazzy-xacro \
    ros-jazzy-robot-state-publisher \
    ros-jazzy-joint-state-publisher \
    ros-jazzy-joint-state-publisher-gui \
    ros-jazzy-tf2-tools
```

If you want the Gazebo simulator executable itself and it is not already installed by your configured Gazebo/ROS packages, install the corresponding Harmonic package from the official Gazebo package source:

```bash
sudo apt install gz-harmonic
```

Do not mix this modern Jazzy/Gazebo workflow with instructions written specifically for the old Gazebo Classic `gazebo_ros` stack.

---

# 50. Verify Gazebo

After installation:

```bash
gz sim
```

If the Gazebo graphical interface opens, Gazebo is installed.

You can close it after testing.

You can also check:

```bash
gz sim --help
```

---

# 51. Verify ROS-Gazebo Integration

Source ROS:

```bash
source /opt/ros/jazzy/setup.bash
```

Check:

```bash
ros2 pkg list | grep ros_gz
```

You should see packages such as:

```text
ros_gz_bridge
ros_gz_image
ros_gz_sim
```

The exact list can vary depending on the installed package set.

---

# 52. RViz vs Gazebo

These two tools have different jobs.

## RViz2

RViz2 is primarily a **visualization tool**.

Think:

```text
ROS data
   |
   v
RViz2
   |
   v
Visual representation
```

For example:

```text
Robot model
TF
Laser scan
Map
Camera
Path
```

---

## Gazebo

Gazebo is a **robot simulation environment**.

Think:

```text
Robot model
     +
Physics
     +
Sensors
     +
Environment
     |
     v
Gazebo simulation
```

Gazebo can simulate things such as:

- Robot movement
- Gravity
- Collisions
- Sensors
- Wheels
- Motors/controllers
- World environments

---

# 53. Recommended Beginner Installation

If this computer is intended for ROS 2 robotics workshops, a useful baseline is:

```bash
sudo apt update

sudo apt install \
    ros-dev-tools \
    python3-colcon-common-extensions \
    ros-jazzy-rviz2 \
    ros-jazzy-ros-gz \
    ros-jazzy-xacro \
    ros-jazzy-robot-state-publisher \
    ros-jazzy-joint-state-publisher \
    ros-jazzy-joint-state-publisher-gui \
    ros-jazzy-tf2-tools
```

Then:

```bash
source /opt/ros/jazzy/setup.bash
```

Verify:

```bash
echo $ROS_DISTRO
```

Expected:

```text
jazzy
```

---

# 54. Final Test Checklist

## ROS 2

Run:

```bash
source /opt/ros/jazzy/setup.bash
```

Then:

```bash
echo $ROS_DISTRO
```

Expected:

```text
jazzy
```

---

## Workspace

Check:

```bash
ls ~/ros_ws
```

You should have:

```text
src
build
install
log
```

after building.

---

## Package

Run:

```bash
ros2 pkg list | grep workshop_demo
```

Expected:

```text
workshop_demo
```

---

## Executables

Run:

```bash
ros2 pkg executables workshop_demo
```

Expected to include:

```text
workshop_demo listener
workshop_demo talker
```

---

## Talker

Terminal 1:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo talker
```

Expected:

```text
Publishing: [5, 7]
```

---

## Listener

Terminal 2:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 run workshop_demo listener
```

Expected:

```text
Received: 5, 7 | Sum = 12
```

---

## Topic

Another terminal:

```bash
source /opt/ros/jazzy/setup.bash
source ~/ros_ws/install/setup.bash
ros2 topic list
```

You should find:

```text
/numbers
```

---

## RViz2

```bash
source /opt/ros/jazzy/setup.bash
rviz2
```

---

## Gazebo

```bash
gz sim
```

---

# 55. One-Page Cheat Sheet

### Start ROS 2

```bash
source /opt/ros/jazzy/setup.bash
```

### Go to workspace

```bash
cd ~/ros_ws
```

### Build

```bash
colcon build --symlink-install
```

### Source your workspace

```bash
source install/setup.bash
```

### Run talker

```bash
ros2 run workshop_demo talker
```

### Run listener

```bash
ros2 run workshop_demo listener
```

### List nodes

```bash
ros2 node list
```

### List topics

```bash
ros2 topic list
```

### Inspect topic

```bash
ros2 topic echo /numbers
```

### Check topic type

```bash
ros2 topic type /numbers
```

### Run RViz

```bash
rviz2
```

### Run Gazebo

```bash
gz sim
```

---

# 56. The Mental Model to Remember

If you remember only one diagram from this tutorial, remember this:

```text
             SYSTEM ROS 2
        /opt/ros/jazzy/
                 |
                 | source
                 v
        Current terminal
                 |
                 v
          ~/ros_ws/src
                 |
          ROS packages
                 |
                 | colcon build
                 v
          ~/ros_ws/build
          ~/ros_ws/install
          ~/ros_ws/log
                 |
                 | source install/setup.bash
                 v
        Current terminal
                 |
                 v
       ROS can find your package
                 |
                 v
     ros2 run workshop_demo talker
                 |
                 v
             /numbers
                 |
                 v
     ros2 run workshop_demo listener
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

This is the workflow you will use repeatedly as you progress from simple ROS 2 nodes to robots, sensors, RViz, Gazebo, TF, URDF, controllers, and eventually more advanced robotics systems.

---

# 57. Official References

- ROS 2 Jazzy installation documentation:  
  https://docs.ros.org/en/jazzy/Installation/

- ROS 2 Jazzy package development:  
  https://docs.ros.org/en/jazzy/How-To-Guides/Developing-a-ROS-2-Package.html

- ROS 2 Jazzy Python package guidance:  
  https://docs.ros.org/en/jazzy/How-To-Guides/Using-Python-Packages.html

- Workshop repository:  
  https://github.com/anyarobotics/ros_workshop

- Gazebo Harmonic ROS 2 documentation:  
  https://gazebosim.org/docs/harmonic/ros2_overview/

The workshop repository's current `setup.py` defines `talker` and `listener` as console-script executables, and its current Python scripts use the `numbers` topic with `Int32MultiArray`. citeturn3view0turn3view1turn3view2
