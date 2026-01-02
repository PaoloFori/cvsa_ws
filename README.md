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

## 🕹️ System Usage & Workflow

The system is operated using ROS launch files located in the external `launchers_bci` repository. The standard operational procedure consists of two main phases: **Calibration** and **Evaluation**.

### 1. Launching the System
All necessary nodes are orchestrated via the launch files. Parameters such as sensor activation (IMU, Eye Tracker) or feedback mode can be toggled directly within these files.

### 2. Workflow Phases

#### Phase A: Calibration
Before full control is enabled, the system usually performs a calibration run to establish a baseline.
* **Feedback:** The user receives continuous audio feedback.
* **Audio Configuration:** The feedback behavior is controlled by the `audio_increasing` flag in the launch file:
    * `true`: Audio intensity/pitch **increases** over time.
    * `false`: Audio intensity **decreases** (or remains static, depending on configuration).

#### Phase B: Evaluation
Once calibrated, the system moves to the evaluation phase where the classifiers (K-Means and QDA) are active and controlling the output.

### 3. Interaction Protocol & Reaction Times
In both the Calibration and Evaluation phases, the user interaction follows a specific timing protocol:

1.  **Cue:** The system provides an initial cue.
2.  **The "Boom" & Visual Target:** A distinct "boom" sound is played. Simultaneously, a visual target (a dot) appears on the monitor in the direction indicated by the cue (Left or Right).
3.  **User Action (Reaction Time):**
    * Upon seeing the visual target/hearing the boom, the user may be required to press the **Spacebar**.
    * A dedicated ROS node is responsible for recording these reaction times.
4.  **Timing Constraint:**
    * The valid window to register a reaction is limited to the **duration of the boom sound**.
    * **Default Max Duration:** `1.5 seconds`.
    * If the spacebar is not pressed within this timeframe, the trial is marked as a "miss" or max time.

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