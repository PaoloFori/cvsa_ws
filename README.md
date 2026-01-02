# Covert Attention BCI System (Audio Feedback)

![ROS Noetic](https://img.shields.io/badge/ROS-Noetic-blue)
![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-MIT-yellow)

This project implements a **Brain-Computer Interface (BCI)** based on the **Covert Attention** paradigm. The system enables the user to modulate brain activity by focusing attention on a peripheral visual stimulus without moving their eyes (covertly).

The system provides continuous **auditory feedback** that increases in intensity/pitch based on the classifier's confidence in detecting the user's intent.

## 🧠 System Architecture

The system processes EEG signals in real-time through the following pipeline:

* **Acquisition:** Real-time EEG data streaming (compatible with rosneuro drivers).
* **Feature Extraction:** Computation of **Log Band Power** features, specifically focused on the **occipital cortex** (channels O1, O2, Oz, POz, etc.) to capture visual responses.
* **Classification:** A **QDA (Quadratic Discriminant Analysis)** classifier is used to discriminate between "resting" state and "attention" state (or different attention classes).
* **Feedback:** An audio tone generation module. The audio signal increases (in volume or pitch) as the system detects the target attention pattern with higher certainty.

## 📦 Repository Structure

The workspace is organized into several ROS submodules. The main packages are:

* `launchers_cvsa`: Contains the `.launch` files to start the calibration and evaluation phases.
* `processing_cvsa`: Nodes for signal processing (LogBand feature extraction).
* `qda_cvsa`: Implementation of the QDA classifier.
* `feedback_cvsa`: Audio feedback management.
* `analysis_cvsa`: Scripts for offline data analysis.

## 🚀 Installation

Ensure you have **ROS Noetic** installed.

1.  Clone the repository recursively to fetch all submodules:
    ```bash
    git clone --recursive git@github.com:PaoloFori/your_mother_repo.git
    cd your_mother_repo
    ```

2.  Build the workspace:
    ```bash
    catkin_make
    source devel/setup.bash
    ```

## 🕹️ Usage

The operational workflow is divided into two distinct phases: **Calibration** and **Evaluation**.

### Phase 1: Calibration (Training)

In this phase, the user performs specific tasks (e.g., following a visual cue) to allow the system to acquire labeled data and train the QDA classifier.

1.  Start the calibration launcher:
    ```bash
    roslaunch launchers_cvsa calibration.launch
    ```
2.  Follow the instructions on the screen. The system will record EEG data associated with the stimuli.
3.  Upon completion, the QDA classifier will be automatically trained and saved.

### Phase 2: Evaluation (Online Feedback)

Once the model is trained, you can proceed to the real-time testing phase with active audio feedback.

1.  Start the evaluation launcher:
    ```bash
    roslaunch launchers_cvsa evaluation.launch
    ```

2.  **Audio Feedback Behavior:**
    * The system begins processing data in real-time.
    * If the system detects that the user is correctly performing the covert attention task, you will hear a **gradually increasing sound** (volume or frequency).
    * Low or absent sound indicates that the system is not detecting the target intent or that the user is in a resting state.

## 🔧 Configuration

System parameters (EEG channels, frequency bands, audio settings) can be modified in the configuration files located in `launchers_cvsa/config`.

* **EEG Channels:** Defined in the `rosneuro_filters` config or the QDA configuration.
* **Frequency Bands:** Configurable in the `processing_cvsa` node (default: Alpha/Beta bands).

## 👥 Authors

* **Paolo Fori** - *Maintainer & Developer*
* **NeuroRobotics IAS-Lab** - *Original packages core*