# EECE 5550 — Mobile Robotics

**Northeastern University · Seattle Campus**  
**Department of Electrical and Computer Engineering**  
Instructor: [Dr. Xian Li](https://shawnlinu.github.io) | x.li1@northeastern.edu

---

## Course Description

A mobile robot is not a machine you program — it is a machine you teach to reason about an uncertain world. It does not know where it is. It does not know what its map looks like. It does not know whether the obstacle in front of it is a wall, a chair, or a person who is about to step aside. Everything it believes about its environment is a probability distribution, and everything it does is a decision made under that uncertainty.

The algorithms that allow robots to navigate despite this uncertainty — probabilistic sensor fusion, simultaneous localization and mapping, path planning, and real-time perception — are what this course is about. They are also what power every autonomous vehicle on public roads, every warehouse robot carrying shelves at Amazon, every delivery robot crossing a sidewalk, and every surgical system navigating a body cavity.

What makes this course different is the platform. Rather than running algorithms on simulation alone, you will deploy them on a custom-built distributed robotic system: a TurtleBot 4 modified with a Raspberry Pi 5 and a Hailo AI accelerator, connected by Ethernet, running SLAM, autonomous navigation, and real-time object detection simultaneously — all on-robot, all without a laptop. In Week 1, you build the platform. By Week 15, it completes a full autonomous mission without you touching a keyboard.

---

## Course Objectives

By the end of EECE 5550, you will be able to:

- **Model** the kinematics of a differential-drive robot and derive probabilistic sensor models for LiDAR, IMU, and wheel encoders
- **Implement** a Bayes filter, Extended Kalman Filter, and particle filter for robot state estimation — in Python and on real hardware using `robot_localization`
- **Build** an occupancy grid map using SLAM Toolbox from live LiDAR data, evaluate its quality, and save it for use in autonomous navigation
- **Configure and operate** the Nav2 autonomous navigation stack — global planner, local controller, AMCL localization, and behavior trees — on a physical TurtleBot 4
- **Design** custom behavior trees that implement multi-waypoint patrol, battery-aware docking, and obstacle-triggered pause-and-resume behaviors
- **Deploy** YOLOv8 object detection on the Hailo-8L neural processing unit at 30+ FPS while SLAM and Nav2 run concurrently, and measure the latency budget of the full stack
- **Integrate** perception and navigation: stop the robot when a person is detected in the path, confirm navigation checkpoints using ArUco markers, and locate a target object by color
- **Engineer** a distributed ROS2 system across two physical computers, configure CycloneDDS for reliable multi-machine communication, and diagnose failures across machine boundaries

---

## Resources

### Recommended Textbooks

The course does not require purchasing a textbook. All required material is on the [course wiki](https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki). The following references are used throughout and are available through the Northeastern University library:

- Thrun, S., Burgard, W., & Fox, D. *Probabilistic Robotics*. MIT Press, 2005. — the foundational reference for Chapters 2–9 of this course. A free PDF is available at [probabilistic-robotics.org](https://www.probabilistic-robotics.org/)
- Macenski, S. et al. "SLAM Toolbox: SLAM for the dynamic world." *Journal of Open Source Software*, 2021. — the primary reference for Lab 4
- LaValle, S. *Planning Algorithms*. Cambridge University Press, 2006. — free at [lavalle.pl/planning](http://lavalle.pl/planning/). Chapters 5–6 cover path planning
- ROS2 Humble documentation: [docs.ros.org/en/humble](https://docs.ros.org/en/humble/)
- Nav2 documentation: [docs.nav2.org](https://docs.nav2.org/)

### Software

| Tool | Purpose |
|------|---------|
| ROS2 Humble | Middleware for all inter-node communication, sensor streaming, and navigation |
| SLAM Toolbox | Online synchronous graph-based SLAM for map building |
| Nav2 | Global + local path planning, AMCL localization, behavior tree execution |
| robot_localization | EKF sensor fusion of odometry and IMU |
| HailoRT + hailo-rpi5-examples | YOLO inference on the Hailo-8L NPU |
| Python 3.10+ (`rclpy`, `numpy`, `opencv-python`, `scipy`) | All student-written ROS2 nodes and homework |
| CycloneDDS (`rmw_cyclonedds_cpp`) | Multi-machine ROS2 communication locked to the Ethernet interface |
| Ubuntu 24.04 (Pi 5) / Ubuntu 24.04 (TurtleBot 4 Pi 4) | Operating systems |

See [Prereq 1: Software Setup](https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki/Prereq-1-Software-Setup) for complete installation instructions.

### Hardware

| Hardware | Role |
|----------|------|
| [TurtleBot 4](https://clearpathrobotics.com/turtlebot-4/) (Clearpath / iRobot) | Mobile base — provides LiDAR, RGB-D camera, wheel odometry, IMU, and Create 3 drive system |
| [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/) (8 GB) | Intelligence node — runs SLAM, Nav2, and object detection; mounted on TurtleBot 4 top shelf |
| [Raspberry Pi AI HAT+](https://www.raspberrypi.com/products/ai-hat/) (Hailo-8L, 13 TOPS) | Neural processing unit — offloads YOLO inference from CPU; connected via PCIe Gen 3 × 1 |
| USB-C PD power banks (× 2) | Extended battery — one supplements the Create 3 battery, one powers the Pi 5 independently |
| Cat 6 flat Ethernet patch cable (0.3 m) | Direct wired link between TurtleBot 4 Pi 4 and Pi 5 for reliable ROS2 communication |

All hardware is provided by the course. Students are responsible for the equipment throughout the semester. See [Lab 0: Hardware Modification](https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki/Lab-0-Hardware-Modification) for the complete assembly guide.

---

## Repository Structure

```
EECE5550-Mobile-Robotics/
│
├── code/
│   ├── nodes/              # ROS2 Python nodes for all labs
│   │   ├── obstacle_stop.py
│   │   ├── yolo_hailo_node.py
│   │   ├── nav_perception_integration.py
│   │   └── mission_controller.py
│   ├── configs/            # SLAM Toolbox, Nav2, EKF, CycloneDDS config files
│   │   ├── slam_params.yaml
│   │   ├── nav2_params.yaml
│   │   ├── ekf.yaml
│   │   └── cyclonedds_eth.xml
│   ├── launch/             # ROS2 launch files for labs and competition
│   ├── behaviors/          # Behavior tree XML files for Lab 6 and competition
│   └── homework/           # Starter notebooks for HW 1 (particle filter) and HW 2 (A*)
│
├── models/
│   └── hef/                # Pre-compiled Hailo .hef model files for YOLO variants
│
└── README.md
```

All lesson pages, lab procedures, homework specifications, and the competition rules live on the **[course wiki](https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki)**.

---

## Course Wiki

**[https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki](https://github.com/ShawnLiNU/EECE5550-Mobile-Robotics/wiki)**

The wiki is the primary course reference. 31 pages covering:

- Three prerequisite setup guides (ROS2 on Pi 5, TurtleBot 4 bring-up, Hailo driver installation)
- A hardware modification guide (Lab 0) with a 16-item completion checklist
- 10 lessons with full derivations, equations, and Python code examples
- 8 lab guides with step-by-step procedures, complete runnable code, expected outputs, and troubleshooting tables
- 2 homework problem sets with multi-part Python implementation exercises
- The full competition specification — scoring rubric, technical requirements, and competition-day schedule
- Quick-reference pages for ROS2 commands and Nav2 parameter tuning
- A comprehensive troubleshooting guide covering the distributed platform, navigation failures, SLAM issues, and the Hailo AI HAT

Pages are added as the semester progresses — check back regularly.

---

## Grading at a Glance

| Component | Weight |
|-----------|--------|
| Lab reports (8 labs) | 30% |
| Homework (2 sets) | 15% |
| Midterm exam | 20% |
| Final project (autonomous mission + report) | 35% |

The final project grade includes competition performance (25%), system architecture and code quality (50%), and the written technical report (25%).


---

*"No guts, no glory. No fun, no story." — R. G. Nuranen*
