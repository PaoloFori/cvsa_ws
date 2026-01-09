# CVSA & Motor Imagery Brain-Computer Interface

This repository contains the main project for a Brain-Computer Interface (BCI) designed to handle both **Covert Visuospatial Attention (CVSA)** and **Motor Imagery (MI)** paradigms. The system aims to classify user intent to control external devices or virtual interfaces.

---

## 📋 Overview

The system architecture is capable of supporting two distinct BCI paradigms via specialized pipelines:

1.  **CVSA (Covert Visuospatial Attention):** Classifies the user's attention (left vs. right focus).
2.  **MI (Motor Imagery):** Classifies the user's imagined movement (left vs. right).

Both pipelines utilize **Gaussian Mixture Models (GMM)** for classification tasks, offering a robust statistical approach to intent detection.

---

## 🤖 System Architecture

### Core Hardware & Interfaces
* **EEG Headset:** `antenuro`
* **Control Targets:**
    * `UR5` robotic manipulator (optional).
    * **Virtual Wheel:** A visual feedback interface specifically designed for Motor Imagery.
    * On-screen monitor (simple visual cues).

### Optional Hardware
The system allows for modular activation of peripherals:
* `IMU` (Inertial Measurement Unit)
* `eye_detector` (Eye-tracking sensor)

---

## 🚀 Launchers & Pipelines

The system is operated using ROS launch files located in the external `launchers_bci` repository. The specific paradigm and algorithm are selected by the suffix of the launch file used:

### 1. CVSA Pipeline (`_cvsa`)
* **Launcher Suffix:** `*_cvsa.launch`
* **Algorithm:** CVSA pipeline utilizing **GMM** (Gaussian Mixture Model).
* **Goal:** Detects spatial attention.

### 2. Motor Imagery Pipeline (`_mi`)
* **Launcher Suffix:** `*_mi.launch`
* **Algorithm:** Motor Imagery pipeline utilizing **GMM**.
* **Goal:** Detects imagined motor commands to control the **Virtual Wheel**.

---

## 📡 Feedback Mechanisms & Protocols

The feedback provided to the user changes depending on the active paradigm (CVSA or MI) and the experimental phase.

### A. CVSA Feedback (Audio + Visual)
In the CVSA paradigm, the system relies on a combination of audio cues and visual targets.

1.  **Audio Feedback:**
    * Continuous audio is played during the trial.
    * **Calibration Configuration:** You can select the type of audio via the launch file parameters:
        * `pink_noise`: Standard noise for concentration.
        * `silent`: Uses a `silent.mp3` file to mute feedback during specific calibration runs if needed.
2.  **Visual Target:**
    * A **dot** appears in the cued zone (Left or Right) indicating where the user should focus.

### B. Motor Imagery Feedback (Visual)
For MI, the system uses a **Virtual Wheel**:
* **Behavior:** The wheel rotates Left or Right based on the system's real-time classification of the user's motor imagery.

---

## ⏱️ Reaction Time (CVSA)

A dedicated ROS node can be optionally launched to measure the user's reaction time during CVSA tasks.

* **Trigger:** The timer starts the moment the visual **dot** appears on the screen.
* **Action:** The user must press the **Spacebar**.
* **Measurement:** The node records the delta between the visual stimulus and the key press.

> **⚠️ CRITICAL NOTE ON CALIBRATION:**
> If using audio feedback during calibration, be aware that the audio track might stop exactly when the visual dot appears.
>
> * **Risk:** The user might subconsciously learn to press the spacebar based on the *cessation of sound* rather than the visual cue.
> * **Consequence:** This allows the user to perform the timing task without actually engaging in the CVSA mental task, potentially invalidating the calibration data.

---

## 🧪 Testing & Analysis

The repository includes a `test` folder containing resources for validation and development.

* **Pseudo-Online Analysis:** Code is provided to run "pseudo-online" analysis using pre-recorded real data.
* **Comparison:** This allows for direct comparison between the **ROS** implementation and **Matlab** prototypes to ensure algorithmic consistency and performance verification.
* **Unit Tests:** Various tests for individual nodes and logic blocks.

---

## 🚀 Modularity & Configuration

The entire system is managed via ROS launch files, offering significant flexibility.

* **Feedback:** The `UR5` robot or `Virtual Wheel` can be enabled/disabled via arguments.
* **Sensors:** Peripheral nodes (`imu`, `eye_detector`) can be easily toggled.

---

## Download

```bash
git clone -b ic_cvsa --recursive git@github.com:PaoloFori/cvsa_ws.git
```