# Vision-Guided Robotic Manipulation with Language Task Planning

> Built at **IGUS Robotics** — an end-to-end system that lets you tell a robot what to do in plain English, and it does it.


## Overview

This project implements a full perception-to-action pipeline on the **IGUS ReBeL 6-DOF cobot**, combining:

- **6D Object Pose Estimation** — real-time object detection and 6D pose from depth camera input (FoundationPose)
- **Motion Planning** — collision-free trajectory generation via MoveIt2
- **Language Task Planning** — LLM-based decomposition of natural language instructions into executable robot actions

The system enables commands like *"pick up the red cup and place it on the left"* to be automatically parsed, planned, and executed on the physical robot.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Natural Language Input                     │
│              "pick the red cup, place it left"               │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────────┐
│                   LLM Task Decomposer                        │
│         Breaks language → sequence of subtasks               │
│     [detect(red_cup), pick(red_cup), place(left_zone)]       │
└──────────────────────┬──────────────────────────────────────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
   ┌────────────┐ ┌──────────┐ ┌──────────────┐
   │ Perception │ │ Planning │ │  Execution   │
   │            │ │          │ │              │
   │ RealSense  │ │ MoveIt2  │ │ ReBeL Cobot  │
   │ + Pose Est │ │ Planner  │ │ Controller   │
   └────────────┘ └──────────┘ └──────────────┘
```

## Results

| Metric | Value |
|--------|-------|
| Grasp success rate (novel objects) | 92% |
| Task setup time vs manual programming | -70% |
| Inference latency (pose estimation) | ~45ms |
| Planning + execution cycle | <3s |

## Tech Stack

- **Robot:** IGUS ReBeL 6-DOF Cobot
- **Middleware:** ROS2 Humble, MoveIt2
- **Perception:** Intel RealSense D435, FoundationPose, PyTorch
- **Language:** OpenAI API (GPT-4) for task decomposition
- **Simulation:** Gazebo, RViz2

## Project Structure

```
vision-guided-robotic-manipulation/
├── src/
│   ├── perception/          # Camera pipeline, pose estimation
│   ├── planning/            # MoveIt2 motion planning
│   ├── language/            # LLM task decomposition
│   ├── control/             # Robot controller interface
│   └── teleoperation/       # Human demonstration recording
├── config/                  # ROS2 launch files, URDF
├── models/                  # Trained weights, configs
├── demos/                   # Demo videos and GIFs
├── docs/                    # Architecture docs
└── README.md
```

## Setup

### Prerequisites
- Ubuntu 22.04
- ROS2 Humble
- Python 3.10+
- IGUS ReBeL with iRC controller
- Intel RealSense D435


## Demo

<!-- Replace with actual GIF/video when available -->
*Demo video coming soon — showing the full language-to-action pipeline in operation.*

## Acknowledgments

Built at the IGUS Robotics Lab. Thanks to Michael Spaziano for providing access to the ReBeL platform and workspace.



