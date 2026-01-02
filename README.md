# CVSA-Based Brain-Computer Interface

This repository contains the main project for a Brain-Computer Interface (BCI) based on CVSA. The primary goal of this system is to classify a user's attention (left vs. right focus) to control an external device, such as a robotic arm or an on-screen interface.

##  Overview

The system architecture relies on a two-stage classification strategy. Instead of continuously classifying user intent, the system first determines if the user is in a state of **"Intentional Control" (IC)** or **"Non-Intentional Control" (NIC)**.

A second classifier, trained specifically for directional focus, is then engaged *only* when the user is in the IC state.

## 🤖 System Architecture

### Core Hardware
* **EEG Headset:** `antenuro`
* **Control Target:** `UR5` robotic manipulator (optional) or an on-screen monitor.

### Optional Hardware
The system is designed to be modular and can be launched with or without the following peripherals:
* `IMU` (Inertial Measurement Unit)
* `eye_detector` (Eye-tracking sensor)

---

## 🧠 Classification Workflow

The BCI's logic is divided into two distinct steps:

1.  **Step 1: State Classification (K-Means)**
    * **Purpose:** To determine the user's cognitive state.
    * **Method:** A **K-Means** clustering model analyzes incoming EEG trials.
    * **Output:** Each trial is classified as either `Intentional Control (IC)` or `Non-Intentional Control (NIC)`.

2.  **Step 2: Intent Classification (QDA)**
    * **Purpose:** To classify the user's specific directional focus.
    * **Method:** A **QDA (Quadratic Discriminant Analysis)** classifier, which has been specifically trained on data from the IC state.
    * **Activation:** This classifier is **only activated** when the K-Means model reports an `IC` state.
    * **Output:** Classifies the user's attention as `left` or `right`.

---

## 🚀 Modularity & Configuration

The entire system is managed via ROS launch files, offering significant flexibility.

* **Feedback:** The `UR5` robot node can be omitted from the launch file. If disabled, the system will default to providing simple feedback on a monitor.
* **Sensors:** Other peripheral nodes, such as `imu` and `eye_detector`, can also be easily enabled or disabled via launch file arguments.

---

## Download
```
git clone -b ic_cvsa --recursive git@github.com:PaoloFori/cvsa_ws.git
```