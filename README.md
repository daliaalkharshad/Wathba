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

## Problem Statement

Two athletes can achieve different race times because of differences in biomechanical characteristics such as:

- Step frequency
- Step and stride length
- Ground contact time
- Flight time
- Knee mechanics
- Trunk position

Knowing that one athlete is slower than another does not explain **which biomechanical factors are contributing to the performance difference**.

Traditional biomechanical analysis often requires specialized laboratories, motion-capture systems, force platforms, and high-speed cameras. Although these methods provide highly detailed measurements, they may not always be accessible for frequent athlete assessment.

WATHBA explores a more accessible video-based approach using artificial intelligence and computer vision to investigate the biomechanical factors behind sprint performance.

---

## Project Objective

The main objective of WATHBA is to support the reduction of the performance gap between developing athletes and elite or Olympic-level sprinters by identifying and explaining the biomechanical factors that may contribute to differences in sprint performance.

The system aims to:

- Analyze sprint videos using computer vision.
- Detect and track multiple athletes.
- Estimate 17 human body keypoints.
- Extract biomechanical performance indicators.
- Build an individual biomechanical profile for each runner.
- Compare athlete measurements with elite-level reference values.
- Identify potential factors contributing to the performance gap.
- Provide interpretable performance insights for athletes and coaches.

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
```

---

## Dataset

A custom sprint pose dataset was collected from multiple sprint-video sources and manually annotated using **Roboflow**.

The dataset was specifically prepared to adapt the pose estimation model to sprint-specific body positions and race conditions.

| Dataset | Images |
|---|---:|
| Original Images | 516 |
| After Augmentation | 1,442 |
| Training | 1,153 |
| Validation | 144 |
| Testing | 145 |

Each athlete is represented using **17 body keypoints**.

---

## Pose Estimation Model

WATHBA uses **YOLO11x-Pose** as its pose estimation model.

### Model Selection

The project initially experimented with **YOLO11n-Pose** as a lightweight model for early pipeline development.

However, WATHBA requires more than basic person detection. The biomechanical calculations depend directly on accurate and stable body keypoints across consecutive video frames.

For this reason, the higher-capacity **YOLO11x-Pose** was selected to prioritize pose quality and keypoint reliability over inference speed.

### Baseline Model

The COCO-pretrained **YOLO11x-Pose** was used as the baseline model.

When applied to sprint race videos, two main limitations were observed:

- Some runners were not consistently detected.
- Keypoints showed instability across consecutive frames.

These limitations are particularly important for WATHBA because unstable or missing keypoints can affect downstream biomechanical measurements.

### Fine-Tuned Model

YOLO11x-Pose was fine-tuned using the custom WATHBA sprint dataset.

The goal of fine-tuning was to adapt the model to sprint-specific conditions such as:

- Rapid limb movement
- Sprint-specific body positions
- Multiple athletes
- Athlete overlap
- Partial occlusion
- Race-video viewpoints

After fine-tuning, runner detection became more consistent and the predicted keypoints showed improved stability across consecutive frames.

---

## Model Evaluation

The baseline and fine-tuned models were evaluated using pose estimation metrics.

| Metric | Baseline | Fine-Tuned | Improvement |
|---|---:|---:|---:|
| **Precision** | 95.58% | **99.52%** | +3.94 pp |
| **Recall** | 92.86% | **98.71%** | +5.85 pp |
| **mAP@50** | 92.50% | **99.48%** | +6.98 pp |
| **mAP@50–95** | 56.31% | **86.38%** | **+30.08 pp** |

The largest improvement was observed in **mAP@50–95**, which increased from **56.31% to 86.38%**.

This improvement indicates stronger pose localization under stricter evaluation thresholds and provides a more reliable foundation for the biomechanical analysis stage.

> **Note:** These results evaluate the pose estimation model. They should not be interpreted as the accuracy of the biomechanical measurements themselves.

---

## Athlete Tracking

WATHBA combines pose estimation with **ByteTrack** to track multiple athletes across consecutive video frames.

Each detected athlete is assigned a unique:

```text
Runner ID
```

This allows biomechanical measurements to be calculated and stored independently for each runner throughout the analyzed sequence.

---

## Biomechanical Analysis

After pose estimation and athlete tracking, WATHBA converts the detected body movement into biomechanical performance indicators.

The primary measurements include:

| Metric | Description |
|---|---|
| **Step / Stride Frequency** | Represents the athlete's running rhythm |
| **Step / Stride Length** | Represents the distance covered during the running cycle |
| **Ground Contact Time** | Time the foot remains in contact with the ground |
| **Flight Time** | Time during which the athlete is airborne |
| **Knee Angle** | Describes lower-limb mechanics during sprinting |
| **Trunk Lean** | Describes the athlete's trunk position during running |

Additional normalized and contextual metrics are calculated to support more meaningful athlete comparison.

---

## Performance Gap Analysis

The final objective is not simply to determine whether an athlete is faster or slower.

WATHBA uses the athlete's biomechanical profile to investigate **why the performance difference may exist**.

```text
Athlete Performance
        │
        ▼
Biomechanical Profile
        │
        ▼
Elite / Olympic Reference
        │
        ▼
Identify Differences
        │
        ▼
Explain Potential Contributors
        │
        ▼
Performance Development Insights
```

By comparing the athlete's biomechanical measurements with relevant elite or Olympic-level reference values, WATHBA aims to identify which movement characteristics may be contributing to the observed performance gap.

> **The time shows the gap. The biomechanics help explain the gap.**

---

## Technologies Used

| Area | Technologies |
|---|---|
| **Programming** | Python |
| **Pose Estimation** | YOLO11x-Pose |
| **Tracking** | ByteTrack |
| **Computer Vision** | OpenCV |
| **Deep Learning** | Ultralytics, PyTorch |
| **Data Processing** | NumPy, Pandas |
| **Dataset Annotation** | Roboflow |
| **Development** | Google Colab, GitHub |


---

## WATHBA Team

- **Aljuhara**
- **Dalia**
- **Mohammed**
- **Nawaf**
- **Abdulaziz**

---

<div align="center">

### WATHBA

**From knowing the time to understanding the performance behind it.**

</div>
