# Wathba

### AI-Powered Biomechanical Performance Analysis for Elite Sprinters

---

## Project Overview

**Wathba** is an AI-powered computer vision system designed to analyze and evaluate the biomechanical performance of elite and Olympic-level sprinters.

The system analyzes sprinting videos to detect and track athletes, estimate human body keypoints, and extract biomechanical **Key Performance Indicators (KPIs)**. These measurements are then compared with reference values from elite and Olympic-level sprinters to identify performance strengths, weaknesses, and potential areas for improvement.

The main goal is to transform ordinary race videos into **objective, data-driven performance insights** that can support athletes and coaches in developing more targeted training strategies.

---

##  Problem Statement

The performance development of Saudi elite and Olympic sprinters faces a significant challenge in obtaining objective, detailed, and data-driven insights into an athlete’s running technique and biomechanics. Traditional performance evaluation often focuses primarily on the final race time, which does not fully explain why an athlete achieved a particular result or which technical and biomechanical factors may be limiting their performance.

Advanced biomechanical analysis typically requires specialized laboratories, motion-capture systems, and expensive sensors, making continuous and accessible performance analysis difficult.

---

##  Project Objective

The main objective  is to develop an accessible AI-based system that can:

1. Analyze sprint race videos.
2. Detect and track athletes across video frames.
3. Estimate human body keypoints using pose estimation.
4. Extract biomechanical performance indicators.
5. Calculate a representative value for each KPI rather than relying only on frame-by-frame measurements.
6. Compare athlete measurements with elite and Olympic-level reference values.
7. Identify potential strengths and weaknesses in running technique.
8. Provide objective insights that can support coaches and athletes in performance development.


---

# System Workflow

```text
              Sprint Video
                    │
                    ▼
          Video Frame Extraction
                    │
                    ▼
          Athlete Detection
                    │
                    ▼
           Pose Estimation
                    │
                    ▼
        17 Body Keypoint Detection
                    │
                    ▼
          Athlete Tracking
                    │
                    ▼
      Biomechanical KPI Calculation
                    │
                    ▼
       KPI Aggregation & Filtering
                    │
                    ▼
       Elite/Olympic Benchmarking
                    │
                    ▼
       Performance Assessment
                    │
                    ▼
         Athlete Insights Report
```

---

#  Dataset

## Data Source

The dataset was personally collected from multiple sources, including **YouTube sprinting and running videos**.

The collected videos were converted into individual frames, after which the athlete body keypoints were manually annotated using **Roboflow**.

The dataset was specifically prepared to fine-tune a pose estimation model for **runner-specific keypoint detection**.

### Dataset Summary

| Category                  | Details                            |
| ------------------------- | ---------------------------------- |
| Data Sources              | Multiple sources including YouTube |
| Annotation Tool           | Roboflow                           |
| Annotation Type           | Human Pose / Keypoint Detection    |
| Keypoints                 | 17                                 |
| Original Images           | 516                                |
| Images After Augmentation | 1,442                              |
| Training Split            | 80%                                |
| Validation Split          | 10%                                 |
| Testing Split             | 10%                                 |

---

#  Keypoint Detection

The system uses **17 human body keypoints** following the YOLO Pose keypoint format.

These keypoints represent the major anatomical locations required to analyze sprinting movement and calculate biomechanical indicators.

The detected keypoints are used to estimate relationships between body joints and analyze the athlete's running technique.

---

#  Model

**Wathba** uses YOLO11x-Pose as the primary pose estimation model. The model was selected as a baseline because of its strong pose estimation capabilities and ability to detect human body keypoints efficiently in video-based applications.

Two model configurations were evaluated:

Baseline Model — YOLO11x-Pose
Pre-trained YOLO11x-Pose model.
Used without fine-tuning on the custom sprinting dataset.
Serves as the baseline for evaluating pose estimation performance.
Fine-Tuned Model — YOLO11x-Pose
The same YOLO11x-Pose architecture was fine-tuned using the custom sprinting dataset.
The dataset contains manually annotated sprinting images with 17 human body keypoints.
The purpose of fine-tuning was to adapt the model to the specific visual characteristics and poses of sprinting athletes.

---

#  Model Training

The custom dataset was used to fine-tune the pose estimation model.

The training pipeline includes:

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
Train / Validation / Test Split
        │
        ▼
YOLO Pose Fine-Tuning
        │
        ▼
Model Evaluation
        │
        ▼
Best Model Selection
```

### Dataset Split

* **80% Training**
* **10% Validation**
* **10% Testing**

---

#  Model Evaluation

The pose estimation model was evaluated using standard pose estimation metrics to measure the accuracy of athlete detection and body keypoint prediction.

### Evaluation Metrics

* **Precision:** Measures how many predicted keypoints were correct.
* **Recall:** Measures how successfully the model detected the expected keypoints.
* **mAP@50:** Evaluates prediction accuracy at an IoU threshold of 0.50.
* **mAP@50–95:** Provides a stricter evaluation across multiple IoU thresholds.

### Model Performance

| Metric        | Baseline | Fine-Tuned |   Improvement |
| ------------- | -------: | ---------: | ------------: |
| **Precision** |   95.58% | **99.52%** |      +3.94 pp |
| **Recall**    |   92.86% | **98.71%** |      +5.85 pp |
| **mAP@50**    |   92.50% | **99.48%** |      +6.98 pp |
| **mAP@50–95** |   56.31% | **86.38%** | **+30.08 pp** |

---

#  Biomechanical KPIs

One of the main components of Wathba is converting pose keypoints into meaningful **biomechanical Key Performance Indicators (KPIs)**.

The system currently focuses on metrics such as:

| KPI                     | Description                                             |
| ----------------------- | ------------------------------------------------------- |
| **Stride Length**       | Distance covered during each running stride             |
| **Stride Frequency**    | Number of strides performed per unit of time            |
| **Ground Contact Time** | Approximate duration of foot contact with the ground    |
| **Knee Angle**          | Joint angle used to evaluate lower-limb mechanics       |
| **Trunk Lean**          | Forward inclination of the athlete's trunk              |
| **Running Direction**   | Direction of the athlete's movement                     |
| **Movement Dynamics**   | Changes in movement characteristics throughout the race |

The system aggregates measurements across the analyzed sequence to provide **representative KPI values for each athlete**, rather than treating every video frame as an independent result.

---

#  KPI Calculation

The system extracts the 17 body keypoints from each frame and uses their spatial and temporal relationships to calculate biomechanical measurements.

For example:

```text
Keypoints
   │
   ├── Hip
   ├── Knee
   ├── Ankle
   ├── Shoulder
   └── Other body joints
          │
          ▼
     Joint Geometry
          │
          ▼
   Temporal Analysis
          │
          ▼
    Biomechanical KPIs
```

Temporal information across multiple frames is particularly important for metrics such as:

* Stride Frequency
* Stride Length
* Ground Contact Time
* Movement dynamics

---

#  Technologies Used

### Programming

* Python

### Computer Vision

* OpenCV
* YOLO Pose
* Pose Estimation
* Object Tracking

### Machine Learning

* Ultralytics
* PyTorch

### Data Processing

* NumPy
* Pandas

### Dataset Annotation

* Roboflow

### Development Environment

* Google Colab
* GitHub

---


# Installation

Clone the repository:



---

#  Team

**Wathba Team**

Aljuhara 
Dalia
Mohammed 
Nawaf 
Abdulaziz



---
