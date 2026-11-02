#  BEACON
### Beam Evaluation, Acquisition & Coarse Optical Navigation

**A Physics-Grounded Simulation Platform for Free-Space Optical Communications (FSOC)**

[![Smart India Hackathon 2026](https://img.shields.io/badge/Smart%20India%20Hackathon-2026-blue)](https://www.sih.gov.in/)
![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)
![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688?logo=fastapi&logoColor=white)
![License](https://img.shields.io/badge/License-TBD-lightgrey)

**Smart India Hackathon 2026 | Problem Statement: SIH26169 | Theme: Space Technology / Smart Automation**

---

##  Overview

**BEACON (Beam Evaluation, Acquisition & Coarse Optical Navigation)** is a simulation platform designed to model, evaluate, and validate coarse optical alignment and tracking algorithms for Free-Space Optical Communications (FSOC).

FSOC enables high-speed communication by transmitting information through laser beams. However, establishing and maintaining a stable optical link requires extremely precise pointing accuracy. Atmospheric disturbances, satellite vibrations, optical noise, and platform motion can disrupt alignment and cause communication link failures.

Traditional hardware-based testing of Pointing, Acquisition, and Tracking (PAT) systems can be expensive and time-consuming.

BEACON addresses this challenge through a physics-grounded virtual environment that simulates optical tracking, atmospheric disturbances, target motion, and gimbal dynamics. It provides a platform for testing tracking algorithms, evaluating system performance, and diagnosing failures without depending on expensive physical optical test equipment.

The proposed architecture is designed to operate offline and support future integration with hardware-in-the-loop (HIL) testing systems.

##  Problem Statement

**SIH26169 — Coarse Optical Alignment & Tracking Simulator for Free-Space Optical Communications (FSOC)**

The key challenges addressed by BEACON are:

- **Precision alignment:** Laser beams with sub-milliradian divergence require accurate pointing to establish a stable communication link.
- **Atmospheric disturbances:** Scintillation and signal fading can reduce optical signal quality and interrupt tracking.
- **Satellite jitter:** High-frequency platform vibrations can destabilize tracking algorithms.
- **Optical interference:** Sun glints, cloud edges, and background noise can produce false target detections.
- **Expensive validation:** Physical optical testbeds and rate tables introduce significant hardware costs and testing overhead.

BEACON aims to provide a reproducible simulation environment for evaluating optical acquisition and tracking strategies under these challenging conditions.

##  Key Features

### 1. Physics-Grounded Simulation Engine
- Simulates orbital target motion, including low Earth orbit (LEO) passes and spiral trajectories.
- Models atmospheric scintillation using log-normal fading.
- Introduces satellite jitter at 12 Hz, 45 Hz, and 120 Hz.
- Supports high-rate disturbance sub-stepping for more detailed motion simulation.
- Generates dynamic sensor backgrounds and motion-blurred optical observations.

### 2. Optical Sensor Simulation
- Models a virtual camera with a 64 mrad field of view.
- Uses a 512-pixel canvas for the simulated sensor view.
- Incorporates background noise and motion-blur effects.
- Uses sub-pixel intensity centroiding to estimate the detected target position.

### 3. Adaptive Target Tracking
- Implements an Extended Kalman Filter (EKF) architecture for target-state estimation.
- Uses Mahalanobis-distance validation to reject inconsistent detections.
- Applies a validation threshold of γ = 9.21.
- Supports constant-velocity state prediction during temporary measurement loss.
- Aims to maintain tracking through short signal fades and reduce false locks.

### 4. Slew-Saturated Gimbal Control
- Models closed-loop pointing using PID control.
- Incorporates a maximum slew-rate constraint of 25 mrad/s.
- Models first-order motor inertia and actuator lag.
- Uses a SEARCH → TRACK state machine to manage acquisition and tracking.
- Includes an anti-windup strategy to handle actuator saturation.

### 5. Real-Time Mission Dashboard
- Displays the simulated celestial sky and target trajectory.
- Visualizes the camera field of view, tracking reticle, and validation-gate ellipse.
- Plots instantaneous pointing error in microradians.
- Displays signal-to-noise ratio (SNR), lock-retention status, and gimbal slew information.
- Provides telemetry-based explanations of tracking failures.

### 6. Algorithm Comparison
BEACON proposes a synchronized comparison mode for evaluating classical centroid tracking against adaptive EKF-based tracking under identical simulation conditions.

Using reproducible random seeds, the platform is designed to support controlled comparisons across different disturbance scenarios.

### 7. Offline-First Deployment
- Designed for operation without an internet connection.
- Uses locally bundled frontend assets.
- Supports packaging as a desktop application with PyInstaller and pywebview.
- Avoids runtime dependence on external CDNs.
- Uses a decoupled architecture intended to facilitate future hardware integration.

##  Technical Approach

BEACON follows a modular simulation and control pipeline.

```text
                 ┌──────────────────────────┐
                 │     Mission Scenario     │
                 │  LEO Pass / Trajectory   │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │    Simulation Engine     │
                 │ Orbital Motion & Jitter  │
                 │ Atmospheric Disturbances │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │    Virtual Optical       │
                 │         Sensor           │
                 │ Noise & Image Formation  │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │ Detection & Estimation   │
                 │ Centroiding + EKF        │
                 │ Mahalanobis Validation  │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │      Gimbal Control      │
                 │ PID + Slew Constraints   │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │     Mission Dashboard    │
                 │ Telemetry & Diagnostics  │
                 └──────────────────────────┘
```

The pipeline connects simulated environmental conditions to sensor observations, target-state estimation, control decisions, and performance visualization.

##  Technology Stack

| Component | Technologies |
|---|---|
| Programming Language | Python 3.11 |
| Numerical Computing | NumPy, SciPy |
| Computer Vision | OpenCV-headless |
| Backend API | FastAPI |
| Real-Time Communication | WebSockets |
| Frontend Framework | React 18 |
| Styling | Tailwind CSS |
| Visualization | HTML5 Canvas |
| Desktop Application | pywebview |
| Application Packaging | PyInstaller |

The stack is designed to support numerical simulation, optical image processing, real-time telemetry, interactive visualization, and offline desktop deployment.

##  Proposed Architecture

The system is organized into five primary modules.

| Module | Responsibilities | Planned Module Path |
|---|---|---|
| Simulation Engine | Orbital dynamics and environmental disturbances | `sim/disturbances.py` |
| Optical Sensor | Camera simulation, image formation, and noise | `sim/camera.py` |
| Tracking and Estimation | Kalman filtering and target-state estimation | `tracking/kalman.py` |
| Gimbal Control | PID control and actuator constraints | `control/gimbal.py` |
| Mission Interface | Interactive visualization and telemetry | `pywebview` + React |

These module paths reflect the architecture described in the project presentation; the final repository structure may differ.

##  Performance Evaluation

BEACON defines the following proposed evaluation targets.

| Metric | Target |
|---|---|
| Pointing error | Below 50 μrad |
| Lock retention | Above 95% across 50 Monte Carlo runs |
| Reacquisition mean time to recovery (MTTR) | Below 800 ms |
| Disturbance simulation | 12 Hz, 45 Hz, and 120 Hz jitter |
| Sensor field of view | 64 mrad |
| Virtual sensor resolution | 512-pixel canvas |

**Note:** These figures are project evaluation targets, not independently verified performance results. Actual results should be reported after implementation and testing.

### Evaluation Scenarios

The simulator is designed to evaluate performance under:

- Low Earth orbit target motion.
- Archimedean spiral search trajectories.
- Log-normal atmospheric scintillation.
- High-frequency satellite jitter.
- Optical distractors and noisy detections.
- Gimbal slew-rate limitations.
- Temporary loss of optical measurements.
- Target reacquisition following a tracking failure.

### Benchmarking Strategy

The proposed benchmarking workflow includes:

1. Configure a mission scenario and disturbance parameters.
2. Initialize the simulation using a reproducible random seed.
3. Run the selected tracking and control algorithms.
4. Record pointing error, lock retention, SNR, and reacquisition time.
5. Repeat the experiment across multiple Monte Carlo runs.
6. Compare performance across algorithms and disturbance conditions.

##  AI and Control Concepts

BEACON combines estimation, computer vision, control theory, and simulation techniques.

**Extended Kalman Filter (EKF)**

Estimates the target's state from noisy sensor measurements and predicts its position when measurements become temporarily unavailable.

**Mahalanobis-Distance Gating**

Evaluates whether a detected target position is statistically consistent with the predicted state. Detections outside the configured validation gate can be rejected to reduce false locks.

**Centroid-Based Optical Detection**

Estimates the center of an illuminated target from its image intensity distribution, providing position measurements for the tracking system.

**PID Control**

Uses feedback from pointing error to calculate corrective gimbal commands, subject to actuator dynamics and slew-rate limits.

**Monte Carlo Simulation**

Runs repeated experiments under controlled random conditions to evaluate the consistency and robustness of tracking algorithms.

These components are primarily based on estimation, numerical simulation, computer vision, and classical control. The project should not be described as a deep-learning-based system unless a neural-network component is separately implemented.

##  Challenges and Mitigation Strategies

| Challenge | Proposed Mitigation |
|---|---|
| Atmospheric scintillation | Predictive state coasting during temporary measurement loss |
| High-frequency satellite jitter | High-rate disturbance sub-stepping and exposure-time integration |
| Optical distractors | Mahalanobis-distance validation gating |
| Gimbal saturation | Velocity-limited PID control and anti-windup |
| Tracking loss | SEARCH → TRACK state management and reacquisition |
| Restricted evaluation environments | Self-contained desktop packaging with bundled assets |

##  Impact and Benefits

### Reduced Testing Costs
Provides a virtual alternative to some hardware-intensive optical alignment and tracking experiments.

### Faster Algorithm Development
Enables rapid experimentation with different tracking and control configurations without requiring a physical optical testbed for every iteration.

### Reproducible Evaluation
Supports fixed-seed Monte Carlo testing for repeatable experiments and algorithm comparisons.

### Improved Failure Analysis
Uses telemetry and simulated system state to help distinguish between possible tracking failures, including signal fading, false detections, and actuator saturation.

### Future Hardware Integration
The modular architecture is intended to support future hardware-in-the-loop testing and integration with optical gimbal systems.

##  Future Enhancements

Potential extensions include:

- Hardware-in-the-loop integration with physical gimbal controllers.
- Support for additional atmospheric turbulence and fading models.
- More sophisticated multi-model target-state estimation.
- Automated parameter tuning for tracking and control algorithms.
- Expanded benchmarking across multiple orbital trajectories.
- Exportable simulation reports and telemetry datasets.
- Extended optical link-budget and communication-performance analysis.

These are potential future improvements, not claims of completed functionality.

##  Research and References

The project presentation identifies the following references for its technical foundations:

1. **Kaushal, H., & Kaddoum, G. (2017/2018).** *Optical Communication in Space: Challenges and Mitigation Techniques.* IEEE Communications Surveys & Tutorials.  
   [Research literature](https://ieeexplore.ieee.org/)

2. **Andrews, L. C., Phillips, R. L., & Hopen, C. Y. (2001).** *Laser Beam Scintillation with Applications.* SPIE Press.

3. **Hemmati, H. (Ed.). (2006).** *Deep Space Optical Communications.* Wiley-Interscience.

4. **ITU-R Recommendations P.1814 and P.1817.** Optical propagation and optical wireless communication performance evaluation.  
   [International Telecommunication Union](https://www.itu.int/)

5. **Bar-Shalom, Y., & Li, X. R.** *Estimation with Applications to Tracking and Navigation.*  
   Reference for statistical estimation and target-tracking concepts.

These references provide background for optical communication, atmospheric propagation, target tracking, and state estimation. Verify the exact bibliographic details and applicability of each reference before using them in a formal publication.

##  Team

**Team Name:** Dilly Dally Devs  
**Team ID:** 123625  
**Competition:** Smart India Hackathon 2026

##  Getting Started

BEACON's proposed stack includes a Python backend and a React frontend. The following is a starting point for local development; adjust the commands to match the actual repository structure.

### Prerequisites

- Python 3.11
- Node.js and npm
- Git

### 1. Clone the Repository

```bash
git clone https://github.com/Aditi-963/BEACON.git
cd BEACON
```

### 2. Set Up the Python Environment

Create and activate a virtual environment.

**Windows:**

```bash
python -m venv .venv
.venv\Scripts\activate
```

**Linux/macOS:**

```bash
python3.11 -m venv .venv
source .venv/bin/activate
```

Install the project's Python dependencies if a `requirements.txt` file is present:

```bash
pip install -r requirements.txt
```

### 3. Start the Backend

If the FastAPI application entry point is `main.py` and the application object is named `app`, start the development server with:

```bash
uvicorn main:app --reload
```

If your application entry point is different, replace `main:app` with the appropriate module and application name.

### 4. Start the Frontend

If the React application resides in a `frontend` directory:

```bash
cd frontend
npm install
npm run dev
```

Use the scripts defined in the frontend's `package.json` if they differ from these defaults.

### 5. Run the Desktop Application

Desktop packaging is intended to use pywebview and PyInstaller. The launch command and packaging configuration depend on the actual desktop entry point and build files in the repository.

**Important:** The commands above are conditional examples based on the proposed technology stack. Confirm the actual filenames, entry points, and dependency manifests in the repository before running them.

##  License

No license has been specified in the project presentation. Add an appropriate `LICENSE` file before distributing the software.

## 🔗 Project Repository

**GitHub:** https://github.com/Aditi-963/BEACON

---

<p align="center">
  <b>BEACON — Simulating Precision. Enabling Optical Connectivity.</b>
  <br>
  Smart India Hackathon 2026 | Space Technology
</p>
