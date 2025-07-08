# Booster Sim — Dockerized Booster Robotics Webots Simulation

This repository provides a **Dockerized development and simulation stack** for Booster Robotics, including:

- ✅ **Webots** — advanced 3D robot simulator.
- ✅ **Booster Control Runner** — bridges the SDK and Webots simulation.
- ✅ **FastDDS** — real-time DDS middleware for ROS 2.
- ✅ **Booster ROS 2 packages** — integrated example and interface nodes.

---

## 📦 How it works

Large binaries like Webots, worlds, control runners and FastDDS monitor are distributed via **GitHub Releases**, not tracked in the repository.

The `Dockerfile` automatically:

- Pulls these `.zip` files with `curl`.
- Unpacks them in the right directories.
- Installs ROS 2 Humble on **NVIDIA CUDA** runtime for GPU-accelerated OpenGL.
- Removes unnecessary Mesa fallback libraries early to avoid software rendering fallback.

---

## 🐳 NVIDIA Runtime Requirements

This stack runs on:

```
FROM nvidia/cuda:12.3.1-runtime-ubuntu22.04
```

**You must have:**

- A compatible NVIDIA GPU.
- Proper NVIDIA drivers installed on the host for CUDA 12.3+.
- The `nvidia-container-toolkit` and `nvidia-docker` runtime configured.

Test with:

```bash
nvidia-smi
```

Ensure it shows your GPU with no errors.

This guarantees **hardware OpenGL rendering** inside the container (not Mesa).

---

## ✅ Quickstart with Makefile

Use the `Makefile` for a simple workflow:

```bash
# Build the Docker image
make compose-build

# Launch the container stack
make compose-up

# Open a shell inside the running container
make exec

# Shut down and clean up
make compose-down
```

✅ The `compose-up` automatically forwards X11 for GUI (Webots) and uses the NVIDIA runtime.

---

## ⚙️ Manual usage inside container

No automatic entrypoint is defined. Once inside the container, use the pre-set **bash aliases**:

```bash
# Run the 7DOF simulation world:
start-webots-7dof

# Or run the release world:
start-webots

# Start the Control Runner (matching version):
start-runner
start-runner-7dof

# Run SDK client example:
sdk-client
```

These are all defined in the `Dockerfile` and ready to use in `~/.bashrc`.

---

## ✅ FastDDS config

A custom `fastdds_profile.xml` is included and auto-set:

```bash
export FASTRTPS_DEFAULT_PROFILES_FILE=/root/Workspace/booster_sim/fastdds_profile.xml
```

No extra setup needed.

---

## ⚡️ GPU OpenGL troubleshooting

If you see **Webots using Mesa**, double-check:

- You started Docker with `--runtime=nvidia`.
- You passed `/dev/dri` and NVIDIA device nodes.
- You have `xhost +local:docker` to allow GUI access.
- Your host has the proprietary NVIDIA OpenGL drivers (`libGLX_nvidia.so` etc.).

---

## 📌 License and vendor notice

The `.zip` binaries are from Booster Robotics official distribution and provided in GitHub Releases only for **team internal use**.

All intellectual property belongs to Booster Robotics.

---

**© Booster Robotics Simulation — 2025**
