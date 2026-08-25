<p align="center">
  <img src="logo1.jpeg" alt="WATHBA Logo" width="450">
</p>


### AI-Powered Biomechanical Performance Analysis for Sprint Athletes

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![YOLO](https://img.shields.io/badge/YOLO-YOLO11x--Pose-purple)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-green)
![Roboflow](https://img.shields.io/badge/Roboflow-Dataset%20%26%20Annotation-orange)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)

---

WATHBA is a computer vision–based biomechanical analysis system designed to transform sprint videos into measurable, athlete-specific performance insights.

The system combines **pose estimation, multi-athlete tracking, temporal gait analysis, biomechanical feature extraction, normalization, and contextual performance assessment** to analyze sprint mechanics beyond race time alone.

---

## Project Overview

Traditional sprint evaluation often focuses on final race time. While race time measures the outcome, it does not fully explain the biomechanical factors that contributed to that performance.

WATHBA analyzes sprint videos frame by frame to:

- Detect multiple athletes.
- Assign and maintain a unique **Runner ID** for each athlete.
- Estimate 17 anatomical keypoints.
- Track body movement across time.
- Detect gait events such as ground contact and flight phases.
- Extract raw biomechanical measurements.
- Derive normalized biomechanical metrics.
- Contextualize results according to speed, running phase, video quality, and other analysis conditions.

The goal is to provide athletes and coaches with an accessible and data-driven method for understanding sprint mechanics without relying exclusively on laboratory-based motion-capture systems.

---

# Problem Statement

Performance differences between sprinters cannot be explained by race time alone.

Two athletes may achieve similar times while demonstrating different:

- Step frequencies
- Step lengths
- Ground contact times
- Flight times
- Knee mechanics
- Trunk positions
- Left-right asymmetries

Advanced biomechanical assessment is traditionally performed using specialized motion-capture laboratories, force platforms, and high-speed measurement systems.

These systems provide high-quality measurements but may not always be accessible for frequent athlete assessment.

WATHBA explores how **AI-based pose estimation and temporal video analysis** can provide an accessible complementary approach for extracting biomechanical information from sprint footage.

---

# Project Objectives

WATHBA aims to:

1. Analyze sprint videos using computer vision.
2. Detect and track multiple athletes throughout a video.
3. Assign a persistent **Runner ID** to each detected athlete.
4. Estimate 17 human body keypoints.
5. Extract temporal gait events from athlete movement.
6. Calculate raw biomechanical performance measurements.
7. Convert selected measurements into normalized and dimensionless metrics.
8. Add contextual information to support fairer athlete comparison.
9. Compare athlete profiles with relevant sprint performance references.
10. Produce interpretable data that can support athlete and coach decision-making.

---

# System Architecture

```text
                     Sprint Video
                          │
                          ▼
                 YOLO11x-Pose Model
                          │
                          ▼
                Athlete Pose Detection
                          │
                          ▼
                  17 Body Keypoints
                          │
                          ▼
                      ByteTrack
                          │
                          ▼
               Unique Runner IDs
                          │
                          ▼
              Temporal Keypoint Filtering
                          │
                          ▼
                  Gait Event Detection
                          │
                          ▼
              ┌─────────────────────┐
              │  LAYER 1: MEASURE   │
              └──────────┬──────────┘
                         │
                         ▼
                 Raw Biomechanics
                         │
                         ▼
              ┌─────────────────────┐
              │ LAYER 2: NORMALISE  │
              └──────────┬──────────┘
                         │
                         ▼
               Derived Metrics
                         │
                         ▼
            ┌────────────────────────┐
            │ LAYER 3: CONTEXTUALISE │
            └───────────┬────────────┘
                        │
                        ▼
               Athlete Assessment
```

---

# Dataset

## Data Collection

The custom sprint pose dataset was collected from multiple video sources, including publicly available sprint footage.

Videos were converted into individual frames, and athlete body keypoints were manually annotated using **Roboflow**.

The dataset was specifically prepared to fine-tune the pose estimation model for sprint-specific body positions and movement patterns.

## Dataset Summary

| Category | Details |
|---|---|
| Data Sources | Multiple sprint video sources |
| Annotation Tool | Roboflow |
| Annotation Type | Human Pose / Keypoint Detection |
| Keypoints | 17 |
| Original Images | 516 |
| Images After Augmentation | 1,442 |
| Training | 1,153 images (~80%) |
| Validation | 144 images (~10%) |
| Testing | 145 images (~10%) |

---

# Pose Estimation

WATHBA uses the standard **17-keypoint human pose representation** supported by YOLO Pose.

The detected anatomical landmarks include keypoints corresponding to the:

- Shoulders
- Hips
- Knees
- Ankles
- Elbows
- Wrists
- Head and facial landmarks

For sprint biomechanical calculations, the analysis primarily relies on the **shoulders, hips, knees, and ankles**.

Keypoints are processed temporally rather than treating every frame independently. This helps reduce unstable detections and supports gait-event and joint-motion analysis.

---

# Athlete Tracking

Pose estimation identifies athletes within individual frames, but biomechanical analysis requires the system to recognize the same athlete across time.

WATHBA therefore combines pose estimation with **ByteTrack multi-object tracking**.

Each athlete receives a unique:

```text
Runner ID
```

For example:

```text
Runner ID 1
Runner ID 2
Runner ID 3
```

Biomechanical measurements are stored and aggregated independently for each Runner ID.

This enables WATHBA to analyze **multiple athletes within the same sprint video**.

---

# Model

WATHBA uses **YOLO11x-Pose** as its pose estimation architecture.

Two configurations were evaluated.

### Baseline Model

A pre-trained YOLO11x-Pose checkpoint was evaluated without additional training on the custom sprint dataset.

It provides the baseline against which sprint-specific fine-tuning is evaluated.

### Fine-Tuned Model

The same architecture was fine-tuned using the custom sprint pose dataset containing manually annotated 17-keypoint athlete poses.

Fine-tuning aims to improve pose estimation under sprint-specific conditions such as:

- High-speed limb movement
- Running-specific body configurations
- Athlete overlap
- Partial occlusion
- Race footage viewpoints

---

# Model Training

```text
Custom Sprint Dataset
        │
        ▼
Manual Keypoint Annotation
        │
        ▼
Data Augmentation
        │
        ▼
80 / 10 / 10 Split
        │
        ▼
YOLO11x-Pose Fine-Tuning
        │
        ▼
Validation & Testing
        │
        ▼
Best Checkpoint
```

---

# Model Evaluation

The baseline and fine-tuned models were evaluated using pose estimation performance metrics.

| Metric | Baseline | Fine-Tuned | Improvement |
|---|---:|---:|---:|
| **Precision** | 95.58% | **99.52%** | +3.94 pp |
| **Recall** | 92.86% | **98.71%** | +5.85 pp |
| **mAP@50** | 92.50% | **99.48%** | +6.98 pp |
| **mAP@50–95** | 56.31% | **86.38%** | **+30.08 pp** |

The largest improvement was observed in **mAP@50–95**, increasing by approximately **30 percentage points**, indicating substantially better localization performance under stricter evaluation thresholds.

---

# Biomechanical Analysis Framework

WATHBA organizes biomechanical analysis into three layers:

```text
MEASURE  →  NORMALISE  →  CONTEXTUALISE
```

This separates direct video measurements from derived biomechanical variables and the contextual information required to interpret them.

---

## Layer 1 — Measure

The first layer extracts raw biomechanical measurements from pose trajectories and gait events.

| Metric | Unit | Description |
|---|---:|---|
| **Step Frequency** | Hz | Number of steps performed per second |
| **Step Length** | m / px | Distance covered per step |
| **Ground Contact Time** | ms | Duration for which the foot remains in contact with the ground |
| **Flight Time** | ms | Time during which neither foot is detected in ground contact |
| **Knee Angle at Initial Contact** | deg | Knee angle when the foot initially contacts the ground |
| **Minimum Knee Angle** | deg | Minimum detected knee angle during the analyzed movement |
| **Trunk Lean at Initial Contact** | deg | Trunk inclination at initial ground contact |
| **Left Step Time** | s | Temporal duration associated with the left step |
| **Right Step Time** | s | Temporal duration associated with the right step |
| **Running Speed** | m/s | Estimated horizontal athlete speed when spatial calibration is available |

---

## Layer 2 — Normalise

Raw measurements can be influenced by athlete body size and measurement scale.

WATHBA therefore derives normalized or dimensionless metrics for more meaningful comparison.

### Duty Factor

```text
DF = GCT × SF
```

Represents the relationship between ground contact duration and step frequency.

### Contact / Flight Ratio

```text
CFR = GCT / FT
```

Compares time spent in ground contact with time spent in flight.

### Relative Step Length

```text
RSL = SL / h
```

Normalizes step length relative to athlete height.

### Normalized Step Frequency

```text
nSF = SF × √(h / g)
```

Normalizes step frequency using body height and gravitational acceleration.

### Froude Number

```text
Fr = v² / (g × h)
```

Provides a dimensionless representation of running speed relative to body scale.

### Knee Delta

```text
KD = Knee_Touchdown − Knee_Min
```

Measures the change between knee angle at initial contact and the minimum observed knee angle.

### Step-Time Asymmetry

```text
ASYM = |TL − TR| / ((TL + TR) / 2)
```

Quantifies temporal asymmetry between left and right steps.

---

## Layer 3 — Contextualise

The final layer does not directly modify the biomechanical measurements.

Instead, it stores information required to determine **how confidently and under what conditions the results should be interpreted**.

| Context Variable | Purpose |
|---|---|
| **Speed Tier** | Groups athletes according to running-speed context |
| **Valid Steps** | Indicates how many usable gait events support the calculated results |
| **Camera Angle** | Identifies the video viewpoint and determines which measurements are reliable |
| **Video FPS** | Indicates temporal resolution, particularly important for contact and flight measurements |
| **Running Phase** | Distinguishes acceleration from maximum-velocity mechanics |
| **Data Quality** | Indicates the reliability of the available measurements |

### Speed Tiers

The current WATHBA implementation uses three project-level categories:

```text
Competitive
Professional / Elite
World-Class / Record-Level
```

> **Note:** These categories are used by WATHBA as contextual performance groups. They should not be presented as official World Athletics classification thresholds unless the final speed boundaries are validated against an appropriate external reference.

---

# Technologies Used

| Area | Technologies |
|---|---|
| **Programming** | Python |
| **Pose Estimation** | YOLO11x-Pose |
| **Tracking** | ByteTrack |
| **Computer Vision** | OpenCV |
| **Deep Learning** | Ultralytics, PyTorch |
| **Numerical Processing** | NumPy |
| **Data Analysis** | Pandas |
| **Dataset Annotation** | Roboflow |
| **Development** | Google Colab, GitHub |

---

# Installation

Clone the repository:

```bash
git clone <repository-url>
cd Wathba
```

Install the required dependencies:

```bash
pip install ultralytics opencv-python numpy pandas
```

---
# Wathba Team 

**Aljuhara** 

**Dalia** 

**Mohammed**

**Nawaf** 

**Abdulaziz**

