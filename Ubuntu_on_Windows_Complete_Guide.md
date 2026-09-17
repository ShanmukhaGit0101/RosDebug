# Using Ubuntu on a Windows Laptop

**Guide version:** September 2026  
**Ubuntu versions covered:** Ubuntu 22.04 LTS and Ubuntu 24.04 LTS  
**Target:** Windows 10/11 laptops and desktops

---

# 1. Available Options

There are several ways to use Ubuntu on a Windows laptop:

1. **Dual Boot**
2. **WSL 2 (Windows Subsystem for Linux)**
3. **VirtualBox**
4. VMware Workstation
5. **Live USB**
6. Remote Ubuntu
7. Cloud Ubuntu
8. Dedicated Ubuntu-only installation

This guide focuses only on:

- **1. Dual Boot**
- **2. WSL 2**
- **3. VirtualBox**
- **5. Live USB**

For a laptop intended for **ROS 2, Gazebo, RViz, MoveIt 2, ros2_control, GPU simulation, and robotics hardware**, dual boot provides a normal native Ubuntu environment with direct access to the computer's hardware. However, because it is a permanent and relatively lengthy setup, **WSL 2 or VirtualBox may be more convenient when Ubuntu is needed only for the duration of a workshop**.

---

# 2. Quick Comparison

| Feature | 1. Dual Boot | 2. WSL 2 | 3. VirtualBox | 5. Live USB |
|---|---|---|---|---|
| Native Ubuntu | Yes | No | No | Yes while booted |
| Reboot required to switch | Yes | No | No | Yes |
| Performance | Excellent | Very good | Moderate | Good |
| Full Ubuntu desktop | Yes | Available | Yes | Yes |
| GPU access | Native | Supported for suitable workloads | Virtualized | Native |
| USB/robot hardware | Excellent | More complicated | Passthrough needed | Excellent |
| ROS 2 | Excellent | Possible | Possible | Possible |
| Gazebo | Excellent | Can work, but test | Can be slower | Good |
| Safe for experimenting | Medium | High | High | High |
| Permanent installation | Yes | Yes | Yes | Usually no |
| Difficulty | Medium | Easy | Easy–Medium | Easy–Medium |

## Practical Recommendation

**Dual boot is a permanent installation approach and is a relatively lengthy process.** It involves changing the computer's disk layout, installing Ubuntu alongside Windows, configuring the bootloader, and setting up the Ubuntu environment.

If Ubuntu is only needed **until a workshop or short-term training period ends**, it may be more practical to use **WSL 2 or VirtualBox**. These options allow Ubuntu to be used inside Windows without permanently changing the computer's normal Windows installation.

### Choose Dual Boot if:

You want a long-term, native Ubuntu environment and are comfortable with the installation and disk-partitioning process.

### Choose WSL 2 if:

You mainly want Linux development tools while continuing to use Windows applications.

### Choose VirtualBox if:

You want a complete Ubuntu desktop inside Windows without modifying your disk partitions.

### Choose Live USB if:

You want to test Ubuntu before installing it or troubleshoot a computer.

---

# 3. Option 1 — Dual Boot ⭐ RECOMMENDED

## 3.1 What is Dual Boot?

Dual boot means installing Windows and Ubuntu on the same physical computer.

When the laptop starts, you select the operating system:

```text
Computer
   |
   +-- Boot Menu
        |
        +-- Ubuntu
        |
        +-- Windows
```

Only one operating system normally runs at a time.

Example:

```text
Laptop SSD
------------------------------------------------
| EFI | Windows | Ubuntu 24.04 | Recovery |
------------------------------------------------
```

Ubuntu and Windows have separate filesystem areas.

---

## 3.2 Why Dual Boot is Recommended for Robotics

For robotics development, you may need:

```text
ROS 2
Gazebo
RViz
MoveIt 2
ros2_control
Python
C++
USB devices
Serial devices
Cameras
LiDAR
IMU
Microcontrollers
GPU acceleration
```

A native Ubuntu installation gives these applications direct access to the Linux system.

For example:

```text
Laptop Hardware
       |
       v
Ubuntu
       |
       +-- ROS 2
       +-- Gazebo
       +-- RViz
       +-- MoveIt
       +-- Robot Hardware
       +-- GPU
```

This is different from:

```text
Laptop
   |
 Windows
   |
   +-- WSL / VM
        |
        +-- Ubuntu
```

where hardware access is mediated by Windows and/or virtualization.

---

# 4. Advantages of Dual Boot

- Near-native performance
- Native Linux kernel
- Native GPU drivers
- Excellent graphics performance
- Direct USB access
- Good robotics hardware compatibility
- Full Ubuntu desktop
- Suitable for ROS 2
- Suitable for Gazebo
- Suitable for MoveIt
- Suitable for long simulations
- Suitable for physical robot development

---

# 5. Disadvantages of Dual Boot

- You must reboot to change operating systems.
- Disk partitioning must be done carefully.
- Boot configuration can be affected by OS updates.
- BitLocker/device encryption requires attention.
- A backup is strongly recommended before installation.

---

# 6. Before Installing Dual Boot

## 6.1 Back Up Your Data

Back up important files before changing partitions.

Examples:

```text
Documents
Pictures
Videos
Projects
Git repositories
ROS workspaces
Datasets
SSH keys
Configuration files
```

Use an external drive or another reliable backup location.

A second partition on the same SSD is **not a true backup**.

---

# 7. Check UEFI

Press:

```text
Win + R
```

Enter:

```text
msinfo32
```

Look for:

```text
BIOS Mode
```

Modern Windows systems should normally show:

```text
UEFI
```

Ubuntu should normally be installed using the same boot mode.

---

# 8. Check BitLocker

Windows may use BitLocker/device encryption.

Before modifying partitions:

1. Check whether encryption is enabled.
2. Make sure the Windows recovery key is available.
3. Back up important data.
4. Suspend protection if required during the installation process.

Do not start disk modification without knowing where your recovery key is.

---

# 9. Disable Windows Fast Startup

Fast Startup can cause problems when Linux accesses Windows partitions.

Open:

```text
Control Panel
    -> Power Options
        -> Choose what the power buttons do
```

Disable:

```text
Turn on fast startup
```

---

# 10. Create Space for Ubuntu

Open Windows:

```text
Disk Management
```

You can access it through:

```text
Win + X
    -> Disk Management
```

Find the Windows partition, usually:

```text
C:
```

Right-click it and select:

```text
Shrink Volume
```

Leave the resulting space as:

```text
Unallocated
```

Example:

```text
Before:

| EFI | Windows C: | Recovery |

After shrinking:

| EFI | Windows C: | Unallocated | Recovery |
                         ^
                         |
                      Ubuntu
```

Do not format the unallocated space unnecessarily. The Ubuntu installer can use it.

---

# 11. How Much Space Should Ubuntu Get?

Approximate values:

| Usage | Suggested space |
|---|---:|
| Basic Ubuntu | 30–40 GB |
| General development | 60–100 GB |
| ROS 2 + Gazebo | 80–150 GB |
| Robotics + datasets | 150–250+ GB |
| Large simulation/ML workloads | 250+ GB |

If the laptop has a large SSD, allocating more space to Ubuntu is useful for:

- ROS bags
- Gazebo worlds
- datasets
- Docker images
- Python environments
- machine-learning models
- large source repositories

---

# 12. Create an Ubuntu Installation USB

You need:

```text
Ubuntu ISO
USB flash drive
```

Use a reliable tool to create a bootable USB.

The general process is:

```text
Ubuntu ISO
    |
    v
Bootable USB
    |
    v
Laptop
    |
    v
Ubuntu Installer
```

---

# 13. Test Ubuntu Before Installing

Boot from the USB and select:

```text
Try Ubuntu
```

Before installing, test:

```text
[ ] Wi-Fi
[ ] Bluetooth
[ ] Keyboard
[ ] Touchpad
[ ] Display
[ ] Audio
[ ] External monitor
[ ] GPU
```

If important hardware does not work correctly in the Live environment, investigate it before installing.

---

# 14. Install Ubuntu

Start the installer.

When asked about installation type, choose the option that allows Ubuntu to coexist with Windows, or manually use the unallocated space if you understand partitioning.

The result should resemble:

```text
SSD
|
+-- EFI System Partition
|
+-- Windows
|
+-- Ubuntu
|
+-- Recovery
```

After installation, reboot.

You should get a boot menu similar to:

```text
Ubuntu
Advanced options for Ubuntu
Windows Boot Manager
```

---

# 15. Testing the Dual-Boot Installation

Test both systems.

### Test Ubuntu

```bash
lsb_release -a
```

### Test Windows

Select Windows from the boot menu.

Confirm that:

- Windows starts normally.
- Ubuntu starts normally.
- Wi-Fi works.
- Graphics work.
- Sound works.
- The correct GPU is detected.

---

# 16. Option 2 — WSL 2

## 16.1 What is WSL?

WSL means:

> Windows Subsystem for Linux

WSL lets you run a Linux environment inside Windows.

The architecture is approximately:

```text
Windows
   |
   +-- WSL 2
        |
        +-- Ubuntu
```

You do not need to reboot to switch between Windows and Ubuntu.

---

# 17. Why Use WSL 2?

WSL 2 is convenient for:

- Linux command-line tools
- Git
- Python
- C/C++
- GCC
- CMake
- Shell scripting
- Software development
- VS Code
- Linux development alongside Windows

For example:

```text
Windows
|
+-- Chrome
+-- Office
+-- VS Code
|
+-- Ubuntu WSL 2
     |
     +-- bash
     +-- Python
     +-- Git
     +-- GCC
```

---

# 18. Install WSL 2

Open:

```text
PowerShell
```

as Administrator.

Run:

```powershell
wsl --install
```

Restart Windows if requested.

Check WSL:

```powershell
wsl --version
```

Check installed distributions:

```powershell
wsl -l -v
```

---

# 19. Install Ubuntu 24.04 on WSL

For a new installation:

```powershell
wsl --install Ubuntu-24.04
```

Then:

```powershell
wsl -l -v
```

You should see something similar to:

```text
NAME            STATE           VERSION
Ubuntu-24.04    Running         2
```

Start it:

```powershell
wsl -d Ubuntu-24.04
```

---

# 20. WSL Filesystems

Windows drives are visible from Ubuntu.

For example:

```bash
cd /mnt/c
```

Windows C: drive corresponds approximately to:

```text
/mnt/c
```

D: drive:

```text
/mnt/d
```

For Linux-heavy development, it is generally preferable to keep active Linux projects inside the Linux filesystem:

```bash
~/projects/
```

rather than placing everything under:

```text
/mnt/c/...
```

---

# 21. Access Windows Files from WSL

Example:

```bash
cd /mnt/c/Users/<WindowsUser>
```

You can also open the current Linux directory in Windows Explorer:

```bash
explorer.exe .
```

---

# 22. WSL and VS Code

A common setup is:

```text
Windows
   |
   +-- VS Code
         |
         +-- Remote WSL
                |
                +-- Ubuntu
```

The editor runs through Windows while the project tools run inside Ubuntu.

This is a very convenient development workflow.

---

# 23. WSL 2 GPU and GUI

Modern WSL configurations can support:

- Linux GUI applications
- GPU compute for supported hardware/software
- CUDA workloads for supported configurations

However, WSL is not identical to a native Ubuntu installation.

For robotics, test the exact combination of:

```text
ROS 2
Gazebo
RViz
GPU
USB hardware
cameras
serial devices
```

before depending on WSL as the primary robotics environment.

---

# 24. WSL 2 Advantages

- No reboot
- Easy installation
- Excellent Windows integration
- Fast startup
- Good development experience
- Good VS Code integration
- Multiple Ubuntu distributions can coexist
- Convenient for programming

---

# 25. WSL 2 Limitations

- Not identical to native Ubuntu
- Hardware access can require additional configuration
- Some robotics hardware workflows are more complicated
- Linux GUI behavior differs from native Ubuntu
- Some applications may behave differently
- Performance can depend on where files are stored

For a serious physical-robot workstation, native Ubuntu is usually simpler.

---

# 26. Option 3 — VirtualBox

VirtualBox runs Ubuntu as a virtual machine inside Windows.

Architecture:

```text
Laptop
   |
   +-- Windows
        |
        +-- VirtualBox
             |
             +-- Ubuntu VM
```

Ubuntu receives virtual hardware:

```text
Virtual CPU
Virtual RAM
Virtual Disk
Virtual Network
Virtual Graphics
```

---

# 27. Install Ubuntu in VirtualBox

General process:

1. Install VirtualBox on Windows.
2. Download the Ubuntu ISO.
3. Open VirtualBox.
4. Create a new virtual machine.
5. Select Linux/Ubuntu.
6. Allocate RAM.
7. Allocate CPU cores.
8. Create a virtual disk.
9. Mount the Ubuntu ISO.
10. Start the VM.
11. Install Ubuntu.

---

# 28. Suggested VirtualBox Resources

For a laptop with sufficient RAM:

### Basic Ubuntu VM

```text
CPU: 2–4 cores
RAM: 4–8 GB
Disk: 40–60 GB
```

### Development VM

```text
CPU: 4–8 cores
RAM: 8–16 GB
Disk: 80–150 GB
```

The exact values depend on your physical laptop.

Do not allocate nearly all system RAM to the VM because Windows still needs memory.

---

# 29. VirtualBox Advantages

- No dual-boot partition required
- Complete Ubuntu desktop
- Ubuntu runs while Windows is running
- Easy to create and delete VMs
- Snapshots are useful
- Good for Linux learning
- Good for testing
- Isolated environment

---

# 30. VirtualBox Limitations

Because Ubuntu is virtualized:

- CPU performance can be lower
- Disk performance can be lower
- GPU performance is virtualized
- USB devices need passthrough
- Some hardware will not behave like native Linux
- Heavy Gazebo simulations may be slower

For robotics simulation, VirtualBox is better considered a **learning/testing environment** rather than the primary high-performance environment.

---

# 31. Option 5 — Live USB

A Live USB allows Ubuntu to run directly from a USB drive without permanently installing it.

Architecture:

```text
Laptop
   |
   +-- USB
        |
        +-- Ubuntu Live
```

You can select:

```text
Try Ubuntu
```

and use Ubuntu temporarily.

---

# 32. Why Use a Live USB?

Live USB is useful for:

- Testing Ubuntu hardware compatibility
- Testing Wi-Fi
- Testing graphics
- Troubleshooting
- Recovering files
- Testing Ubuntu before dual boot
- Demonstrations
- Temporary Linux use

It is particularly useful before installing Ubuntu permanently.

---

# 33. Live USB Limitations

A normal Live USB is not intended to replace a permanent Ubuntu installation.

Depending on the configuration:

- Changes may disappear after reboot.
- USB storage can be slower.
- Large software installations are inconvenient.
- ROS/Gazebo development is not ideal as a permanent workflow.
- Performance depends heavily on the USB device.

A fast USB 3.x drive or external SSD provides a better experience than a cheap USB flash drive.

---

# 34. Ubuntu 22.04 → Ubuntu 24.04 LTS

If Ubuntu 22.04 is already installed, you can upgrade it to Ubuntu 24.04 LTS.

The process is different depending on whether Ubuntu is:

```text
Native / Dual Boot
```

or:

```text
WSL
```

---

# 35. First Check Your Current Version

Run:

```bash
lsb_release -a
```

or:

```bash
cat /etc/os-release
```

You should currently see something similar to:

```text
Ubuntu 22.04 LTS
```

---

# 36. Back Up Before Upgrading

Before a major Ubuntu upgrade, back up:

```text
Documents
Projects
ROS workspaces
Git repositories
Datasets
SSH keys
Configuration files
Important scripts
```

For a Git project:

```bash
git status
```

Commit important changes:

```bash
git add .
git commit -m "Backup before Ubuntu upgrade"
```

If the repository has a remote:

```bash
git push
```

---

# 37. Update Ubuntu 22.04 First

Before upgrading to 24.04, completely update the existing 22.04 installation.

Run:

```bash
sudo apt update
```

Then:

```bash
sudo apt full-upgrade
```

Remove unnecessary packages:

```bash
sudo apt autoremove
```

Reboot:

```bash
sudo reboot
```

After reboot:

```bash
lsb_release -a
```

Confirm that you are still on a healthy 22.04 installation.

---

# 38. Check for Broken Packages

Run:

```bash
sudo dpkg --configure -a
```

Then:

```bash
sudo apt --fix-broken install
```

Then:

```bash
sudo apt update
sudo apt full-upgrade
```

Fix any package problems before attempting the release upgrade.

---

# 39. Check Third-Party Repositories

This is particularly important on development machines.

Check:

```bash
ls /etc/apt/sources.list.d/
```

You may have repositories for:

```text
NVIDIA
CUDA
ROS
Docker
Google
PPAs
Hardware vendors
```

Do not blindly keep using Ubuntu 22.04 repositories after upgrading to Ubuntu 24.04.

Third-party software may need a 24.04-compatible repository or a fresh installation.

---

# 40. Start the Native Ubuntu Upgrade

For a normal Ubuntu 22.04 installation:

```bash
sudo do-release-upgrade
```

Follow the installer prompts.

The upgrade can:

- Download many packages
- Install new packages
- Remove obsolete packages
- Update system configuration
- Update the kernel
- Update system services
- Modify repository configuration
- Update boot components

Keep the laptop connected to AC power.

Do not forcibly shut down the computer during the upgrade.

---

# 41. Graphical Upgrade

On Ubuntu Desktop, you can also use:

```text
Software Updater
```

First install all normal updates.

Then the system can offer the new LTS release when the upgrade becomes available for your installation/configuration.

---

# 42. After Upgrading to Ubuntu 24.04

Reboot:

```bash
sudo reboot
```

Check:

```bash
lsb_release -a
```

You should see:

```text
Ubuntu 24.04 LTS
```

Check the kernel:

```bash
uname -r
```

Check failed services:

```bash
systemctl --failed
```

Check package state:

```bash
sudo apt update
sudo apt full-upgrade
```

Then:

```bash
sudo apt autoremove
```

---

# 43. Check Hardware After the Upgrade

Check graphics:

```bash
lspci | grep -Ei "vga|3d|display"
```

For NVIDIA:

```bash
nvidia-smi
```

Check network:

```bash
ip a
```

Test internet:

```bash
ping -c 4 8.8.8.8
```

Test DNS:

```bash
ping -c 4 archive.ubuntu.com
```

Also test:

```text
[ ] Wi-Fi
[ ] Bluetooth
[ ] Audio
[ ] Display
[ ] External monitor
[ ] GPU
[ ] USB
```

---

# 44. ROS 2 Users — Important Upgrade Warning

If the Ubuntu 22.04 system contains ROS 2, do not assume that upgrading Ubuntu automatically gives you a correct ROS installation for Ubuntu 24.04.

For example:

```text
Ubuntu 22.04
      |
      +-- ROS 2 Humble
```

is a specific software combination.

After changing to:

```text
Ubuntu 24.04
```

you must check which ROS 2 distribution and packages are appropriate for 24.04.

Before upgrading a ROS workstation, record:

```bash
ros2 pkg list
```

and your workspace contents.

Also back up:

```text
~/ros2_ws
~/your_robot_ws
```

or whatever workspaces you use.

---

# 45. Rebuilding a ROS Workspace After an OS Migration

After a major Ubuntu change, it can be useful to rebuild the workspace rather than trusting old generated files.

Typical process:

```bash
cd ~/ros2_ws
```

Back up the source first.

Then, if appropriate:

```bash
rm -rf build install log
```

Source the installed ROS environment:

```bash
source /opt/ros/<ros-distro>/setup.bash
```

Build:

```bash
colcon build --symlink-install
```

Then:

```bash
source install/setup.bash
```

Replace `<ros-distro>` with the ROS distribution actually installed and supported on the new Ubuntu release.

---

# 46. WSL: Ubuntu 22.04 → 24.04

WSL has its own upgrade procedure.

First check:

```powershell
wsl -l -v
```

For an Ubuntu 22.04 WSL instance, update the existing system:

```bash
sudo apt update
sudo apt full-upgrade
```

Then:

```bash
sudo do-release-upgrade
```

Follow the prompts.

Ubuntu also documents another practical approach: install a new Ubuntu 24.04 WSL instance and migrate your projects.

For example, from PowerShell:

```powershell
wsl --install Ubuntu-24.04
```

Then:

```powershell
wsl -l -v
```

This can leave your old Ubuntu installation untouched while you test the new environment.

---

# 47. WSL Backup Before Migration

A WSL distribution can be exported from PowerShell:

```powershell
wsl --export Ubuntu-22.04 Ubuntu-22.04-backup.tar
```

This provides a backup that can later be imported.

The general import form is:

```powershell
wsl --import <NewName> <InstallLocation> <BackupTar>
```

Example:

```powershell
wsl --import UbuntuBackup C:\WSL\UbuntuBackup C:\Backup\Ubuntu-22.04-backup.tar
```

---

# 48. In-Place Upgrade vs Clean Installation

## In-place upgrade

```text
Ubuntu 22.04
     |
     | do-release-upgrade
     v
Ubuntu 24.04
```

Advantages:

- Existing applications remain installed.
- User files remain in place.
- Less initial configuration.

Possible disadvantages:

- Old configuration can remain.
- Third-party packages can complicate the upgrade.
- Existing problems can carry forward.

---

## Clean installation

```text
Ubuntu 24.04
     |
     +-- Install software again
     +-- Restore projects
     +-- Restore configuration
```

Advantages:

- Clean environment.
- Easier to understand.
- Good for heavily customized systems.

Disadvantages:

- More setup work.
- Applications must be reinstalled.
- Configuration must be restored.

---

# 49. Upgrade Checklist

## Before upgrading

```text
[ ] Back up important files
[ ] Back up ROS workspaces
[ ] Commit Git repositories
[ ] Push important repositories
[ ] Check disk space
[ ] Check third-party repositories
[ ] Check BitLocker if using dual boot
[ ] Save important configuration
[ ] Connect laptop to AC power
[ ] Ensure stable internet
```

## Update 22.04

```bash
sudo apt update
sudo apt full-upgrade
sudo apt autoremove
sudo reboot
```

## Start upgrade

```bash
sudo do-release-upgrade
```

## After upgrade

```text
[ ] Verify Ubuntu 24.04
[ ] Verify kernel
[ ] Verify networking
[ ] Verify graphics
[ ] Verify GPU
[ ] Verify audio
[ ] Verify Bluetooth
[ ] Verify USB
[ ] Verify development tools
[ ] Verify ROS
[ ] Rebuild ROS workspaces if necessary
[ ] Test important projects
```

---

# 50. Final Recommendation

For a Windows laptop that will be used for serious robotics development:

```text
                 Windows Laptop
                       |
             +---------+---------+
             |                   |
          Windows             Ubuntu
                               |
                               |
                         Dual Boot ⭐
                               |
                 +-------------+-------------+
                 |             |             |
                ROS 2        Gazebo        MoveIt
                 |
             Robot Hardware
```

### For a long-term setup: **Dual Boot**

Use **Ubuntu natively through dual boot** when you need:

- ROS 2
- Gazebo
- RViz
- MoveIt 2
- ros2_control
- GPU simulation
- USB devices
- physical robot hardware
- maximum Linux performance

Use **WSL 2** when you mainly need Linux development tools while staying inside Windows. It is a convenient choice for a temporary workshop environment because it does not require repartitioning the disk or changing the boot setup.

Use **VirtualBox** when you want an isolated Ubuntu desktop for learning or testing, especially if you only need Ubuntu temporarily, such as until a workshop ends.

Use a **Live USB** when you want to test Ubuntu before installing it.

---

# 51. Official References

Ubuntu WSL documentation:

https://ubuntu.com/wsl/docs/stable/

Ubuntu WSL 2 installation:

https://ubuntu.com/wsl/docs/latest/howto/install-ubuntu-wsl2/

Ubuntu WSL upgrade:

https://ubuntu.com/wsl/docs/stable/howto/upgrade-ubuntu/

Ubuntu 24.04 LTS release notes:

https://documentation.ubuntu.com/release-notes/24.04/

Ubuntu 22.04 LTS release notes:

https://documentation.ubuntu.com/release-notes/22.04/

Ubuntu community upgrade documentation:

https://help.ubuntu.com/community/NobleUpgrades

---

# Important Safety Note

Disk partitioning, bootloader changes, operating-system installation, and major release upgrades can cause data loss if performed incorrectly.

**Always back up important data before modifying partitions or upgrading the operating system.**
