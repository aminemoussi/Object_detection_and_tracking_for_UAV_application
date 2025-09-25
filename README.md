# Aerial Surveillance Gimbal System

## Overview

A hardware-software system for real-time object detection and tracking using a Raspberry Pi-controlled gimbal. Detects and tracks targets (e.g., vehicles, pedestrians, tanks, armed vehicles, soldiers) in aerial imagery, with integration to a ground control UI. Achieved 90%+ detection accuracy and ~5 FPS inference on Raspberry Pi + Movidus NCS 2, 5-12 FPS on CPU (intel I3 11th gen), and 40 FPS on Nvidia RTX 4060 8G.

![Demo GIF](media/demo.gif)
*demo showcasing the system with the PC configured as the processing unit*

This project was developed as part of a security drone internship at Electronic Systems Research and Development Unit (Feb-Jun 2025), combining AI vision with mechanical control.

## Features
- **Object Detection**: Uses YOLOv8 small or HyperDet CNN models, pre-trained on ImageNet and fine-tuned on Roboflow dataset for custom classes.
- **Tracking**: Implements CSRT and DeepSort for robust, occlusion-resistant tracking.
- **Hardware Integration**: Raspberry Pi 5 with Pi Camera 2 on Storm32 gimbal; sends motor signals for physical tracking.
- **Streaming & UI**: UDP socket video stream to PC; GroundControl UI with sensor panels, error handling, and interactive object selection.
- **Performance**: Optimized for edge devices with Movidius Neural Compute Stick.

## System Architecture
![System Diagram](media/system_diagram.png)
*System overview showing the complete pipeline*

- **Raspberry Pi Side**: Captures frames, controls gimbal via signals, streams video/data via UDP.
- **PC Side**: Receives stream, runs inference, displays UI, allows operator to select/track objects and what models to use.
  
  *P.S: the inference runing unit could be configures to be either the raspberry + NCS 2, PC's CPU or GPU, in this demonstration the CPU was used to obtain the average results*


## Installation
1. Clone the repo: `git clone https://github.com/aminemoussi/Object_detection_and_tracking_for_UAV_application.git`
2. Install dependencies: `pip install -r requirements.txt` (Includes ultralytics, opencv-python, deepsort-realtime, etc.)
3. Download models: Place YOLOv8s.pt and HyperDet weights in `/models/` (links in models/README.md).
4. Hardware setup: Connect Pi Camera to Raspberry Pi 5, gimbal to Storm32 controller, Movidius stick for acceleration.

## Usage
- **Run on Raspberry Pi (Detection & Gimbal Control)**: `python gimbal_tracker.py --model yolov8s.pt --tracker csrt`
- **Run PC UI (Ground Control)**: `python ground_control_ui.py --ip 192.168.247.196 --port 5000`
- Select object in UI to initiate tracking; gimbal adjusts in real-time.


