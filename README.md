<div align="center">

<img src="images/logo 1.jpeg" alt="WATHBA Logo" width="500">

### AI-Powered Sprint Performance Gap Analysis

**Beyond race time — understanding why the gap exists.**

</div>

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![YOLO](https://img.shields.io/badge/YOLO-YOLO11x--Pose-purple)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-green)
![Roboflow](https://img.shields.io/badge/Roboflow-Dataset%20%26%20Annotation-orange)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)

---

# WATHBA

## Project Overview

In sprinting, race time tells us **how large the performance gap is**, but it does not explain **why that gap exists**.

**WATHBA** is an AI-powered sprint analysis system designed to identify the biomechanical factors that may contribute to the performance gap between an athlete and elite or Olympic-level sprinters.

Using standard sprint videos, WATHBA combines pose estimation, athlete tracking, temporal analysis, and biomechanical measurements to transform race footage into athlete-specific performance insights.

> **Race time identifies the gap. WATHBA investigates what is behind it.**

---

## Problem

Two athletes can achieve different race times because of differences in:

- Step frequency
- Step/stride length
- Ground contact time
- Flight time
- Knee mechanics
- Trunk position

Traditional biomechanical analysis often requires specialized laboratories, motion-capture systems, force platforms, and high-speed cameras.

WATHBA explores a more accessible video-based approach using AI and computer vision.

---

## System Workflow

```text
Sprint Video
     │
     ▼
YOLO11x-Pose
     │
     ▼
17 Body Keypoints
     │
     ▼
ByteTrack
     │
     ▼
Runner IDs
     │
     ▼
Temporal Processing
     │
     ▼
Gait Event Detection
     │
     ▼
Biomechanical Metrics
     │
     ▼
Elite / Olympic Comparison
     │
     ▼
Performance Gap Analysis
     │
     ▼
Athlete Insights
