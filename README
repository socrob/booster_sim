# 🚀 Booster Sim — Dockerized Booster Robotics Simulation

This repository contains a **Docker-based development environment** for running the **Booster Robotics simulation stack**, including:

- ✅ **Webots** — for robot simulation.
- ✅ **Booster Control Runner** — to bridge the SDK and Webots.
- ✅ **FastDDS** — configured for ROS 2 inter-process communication.
- ✅ **Booster ROS 2 packages** — for future integration.

---

## 📦 How this works

Some parts of the Booster stack (like the Webots environment, control runner, FastDDS monitor) are large binaries provided directly by Booster Robotics.

To ensure **reproducible, CI/CD-friendly builds**, these `.zip` files are **not tracked** in the git repo but are instead **attached to GitHub Releases**.

The `Dockerfile`:
- Pulls the official binaries directly using `curl` from the GitHub Release.
- Unzips them into the correct locations inside the container.
- Installs ROS 2 Humble with NVIDIA CUDA base for GPU acceleration.

---

## 🐳 NVIDIA Runtime Requirements

**Note:**  
This image uses:

```
FROM nvidia/cuda:12.3.1-runtime-ubuntu22.04
```

👉 **This means your host must have a compatible NVIDIA GPU and matching drivers** for CUDA 12.3.1 and the NVIDIA Container Runtime.  
Test with:
```bash
nvidia-smi
```

---

## ✅ How to run

```bash
# Build the image
make compose-build

# Launch in detached mode
make compose-up

# Open a shell inside the container
make exec

# Shut everything down cleanly
make compose-down
```

---

## ⚙️ Manual startup inside the container

There is **no automatic `ENTRYPOINT`** right now — you manually launch your simulation parts:

```bash
# Inside the container:
start-webots
start-runner
sdk-client
```

These `bash` aliases are pre-configured for you in `.bashrc`.

---

## ✅ FastDDS

A custom `fastdds_profile.xml` is included. The `Dockerfile` sets:
```bash
export FASTRTPS_DEFAULT_PROFILES_FILE=/root/Workspace/booster_sim/fastdds_profile.xml
```

---

## 📌 License and vendor note

The `.zip` files come from the official Booster Robotics documentation.  
We host them in Releases only for **internal CI/CD reproducibility**.  
All rights and ownership belong to Booster Robotics.


---

**© Booster Robotics Internal Simulation — 2024**