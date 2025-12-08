# NVIDIA GPU Offloading Setup — Summary Guide

## 1. Check GPU & Drivers

```bash
nvidia-smi
lsmod | grep nvidia
prime-select query
```

---

## 2. Install Required Tools

```bash
sudo apt install nvidia-prime mesa-utils glmark2
```

---

## 3. Check Default Renderer (iGPU expected)

```bash
glxinfo | grep "OpenGL renderer"
```

---

## 4. Create `prime-run` Script for Offloading

```bash
sudo nano /usr/local/bin/prime-run
```

Paste:
```bash
#!/bin/sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only "$@"
```

Make executable:

```bash
sudo chmod +x /usr/local/bin/prime-run
```

Verify:
```bash
prime-run glxinfo | grep "OpenGL renderer"
```

Expected → `NVIDIA GeForce RTX 4050`

---

## 5. Running Applications with NVIDIA GPU

```bash
prime-run <application>
```

Examples:

```bash
prime-run gazebo
prime-run glmark2
prime-run rviz2
prime-run ros2 launch <pkg> <launchfile>
```

---

## 6. Monitor GPU Usage

```bash
watch -n 1 nvidia-smi
```

---

## 7. Switch Graphics Mode

Full NVIDIA mode:
```bash
sudo prime-select nvidia
sudo reboot
```

Return to hybrid:
```bash
sudo prime-select on-demand
sudo reboot
```

---

## 8. Useful Aliases

Add to `.bashrc`:

```bash
alias gazebo-nvidia='prime-run gazebo'
alias rviz-nvidia='prime-run rviz2'
alias ros2-nvidia='prime-run ros2'
alias glmark-nvidia='prime-run glmark2'
```

Apply:
```bash
source ~/.bashrc
```

Now run:
```bash
gazebo-nvidia
```

---

## Final Summary

| Feature | Status |
|---|---|
| NVIDIA Drivers | ✔ Installed |
| Hybrid PRIME | ✔ Active |
| GPU Offloading | ✔ via `prime-run` |
| Gazebo/ROS Acceleration | ✔ |
| Desktop on iGPU | ✔ Saves battery |

---

Save this .md file as your reference cheat sheet.
