# 🤖 Robotics Assignment — URDF & Xacro Models

**By Haruto**

This repository contains URDF and Xacro descriptions for various robot models, created as part of a Robotics assignment. Each model includes full `<visual>`, `<collision>`, and `<inertial>` definitions and can be visualized in **RViz2** (ROS 2 Humble).

---

## 📁 Models

### 1. Differential Drive Robot — `diff_drive_robot.urdf`

| Property | Value |
|---|---|
| Base | Box 0.60 × 0.40 × 0.20 m |
| Wheels | 2 × Cylinder (r=0.10 m, w=0.05 m) |
| Wheel Separation | 0.40 m |
| Joints | Continuous (Y-axis) |

**TF Tree:** `base_link` → `left_wheel`, `right_wheel`

---

### 2. 2-DOF Robotic Arm — `robo_arm.urdf`

| Property | Value |
|---|---|
| Base | Cylinder pedestal (r=0.06 m, h=0.10 m) |
| Link 1 | Cylinder (r=0.04 m, l=0.40 m) |
| Link 2 | Cylinder (r=0.03 m, l=0.30 m) |
| Joints | Revolute (Z-axis, ±3.14 rad) |

**TF Tree:** `base_link` → `link1` → `link2`

---

### 3. Four-Wheel Service Robot — `four_wheel_robot.urdf`

| Property | Value |
|---|---|
| Base | Box 0.70 × 0.50 × 0.25 m |
| Wheels | 4 × Cylinder (r=0.12 m, w=0.05 m) |
| Placement | Corners (flush with base edges) |
| Joints | Continuous (Y-axis) |

**TF Tree:** `base_link` → `front_left_wheel`, `front_right_wheel`, `rear_left_wheel`, `rear_right_wheel`

---

### 4. Indoor Delivery Robot (Xacro) — `delivery_robot.urdf.xacro`

| Property | Value |
|---|---|
| Base | Box 0.65 × 0.45 × 0.20 m |
| Cargo Box | Box 0.30 × 0.25 × 0.20 m (on top) |
| Drive Wheels | 2 × Cylinder (r=0.10 m, w=0.05 m) |
| Caster | Sphere (r=0.05 m) |
| Lidar | Cylinder (0.55 m above ground) |
| Camera | Box (front face) |
| Battery | Box (centred inside base) |

**TF Tree:**
```
base_link
├── left_wheel        (continuous)
├── right_wheel       (continuous)
├── caster_wheel      (continuous)
├── cargo_link        (fixed)
├── lidar_link        (fixed)
├── camera_link       (fixed)
└── battery_link      (fixed)
```

---

## 🛠️ Prerequisites

```bash
sudo apt update
sudo apt install ros-humble-urdf-tutorial \
                 ros-humble-joint-state-publisher-gui \
                 ros-humble-robot-state-publisher \
                 ros-humble-rviz2 \
                 ros-humble-xacro \
                 liburdfdom-tools
```

---

## 🚀 How to Run

> Source ROS in **every** terminal before running commands:
> ```bash
> source /opt/ros/humble/setup.bash
> ```

### URDF Files (Models 1–3)

**Option A — One-liner** (if path has no spaces):
```bash
ros2 launch urdf_tutorial display.launch.py model:=/home/shashwat/Assig/<filename>.urdf
```

**Option B — Manual (3 terminals):**

| Terminal | Command |
|---|---|
| **1** | `ros2 run robot_state_publisher robot_state_publisher --ros-args -p robot_description:="$(cat /home/shashwat/Assig/<filename>.urdf)"` |
| **2** | `ros2 run joint_state_publisher_gui joint_state_publisher_gui` |
| **3** | `rviz2` |

Then in RViz: **Fixed Frame** → `base_link` → **Add** → **RobotModel** → **Description Topic** → `/robot_description`.

### Xacro File (Model 4 — Delivery Robot)

**Terminal 1:**
```bash
ros2 run robot_state_publisher robot_state_publisher \
  --ros-args -p robot_description:="$(xacro /home/shashwat/Assig/delivery_robot.urdf.xacro)"
```

**Terminal 2:**
```bash
ros2 run joint_state_publisher_gui joint_state_publisher_gui
```

**Terminal 3:**
```bash
rviz2
```

Then in RViz: **Fixed Frame** → `base_link` → **Add** → **RobotModel** → **Description Topic** → `/robot_description`.

### Validate a URDF

```bash
# Plain URDF
check_urdf /home/shashwat/Assig/diff_drive_robot.urdf

# Xacro (convert first)
xacro /home/shashwat/Assig/delivery_robot.urdf.xacro > /tmp/delivery_robot.urdf
check_urdf /tmp/delivery_robot.urdf
```

---

## 📊 TF Tree Diagrams

Pre-generated TF tree visualizations are included as `.gv` (Graphviz), `.pdf`, and `.png` files for each model.

---

## 📝 Notes

- All inertia tensors are computed using **standard density** (ρ = 1000 kg/m³).
- `base_link` origins are placed at **ground level** (z = 0) so robots sit correctly on the ground plane.
- The Xacro file uses **parametric properties** and **reusable macros** for easy modification.
