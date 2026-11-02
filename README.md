# SUNDOG

Project SunDog is a heterogeneous robotic maintenance system designed for utility-scale photovoltaic (PV) arrays. The architecture integrates an aerial scout drone, a physics-based Digital Twin mathematical logic gate, and a two-stage ground deployment system (a delivery rover and a panel-climbing cleaning bot) to automate both fault detection and physical intervention.

The system utilizes a closed-loop data pipeline over a localized MQTT network. It is specifically engineered to mathematically validate thermal anomalies before deploying ground hardware, and to autonomously generate ground-truth training data for its computer vision models based on the results of physical cleaning cycles.

## System Architecture

The architecture operates in four sequential phases, removing human operators from both the deployment authorization and data annotation loops.

### 1. Aerial Scout Node

An aerial drone acts as the primary inspection platform. Equipped with an MLX90640 I2C thermal array and an RGB camera, the scout executes grid flight paths over the PV array. Upon detecting a thermal anomaly (hotspot), a companion computer (Raspberry Pi Zero 2 W) publishes the raw thermal pixel data and GPS coordinates via MQTT to the central edge server.

### 2. Digital Twin Logic Gate

The central edge server runs a mathematical simulation of the solar farm. Instead of acting as a passive dashboard, the Digital Twin computes an active, real-time baseline of expected panel performance based on current irradiance, wind speed, and ambient temperature.

* **Credibility Filter:** The server calculates the mathematical delta between the drone’s raw thermal scan and the Digital Twin's simulated ideal state.
* **Authorization:** If the delta is within the margin of error (e.g., passing shadows, sensor glare), the anomaly is discarded. If the delta exceeds the threshold, the Digital Twin autonomously authorizes and dispatches the ground node.

### 3. Ground Intervention (Delivery & Cleaning)

To respect the 3.2mm structural glass limits and ground clearance of utility-scale PV racks, the physical intervention utilizes a two-stage hardware approach:

* **Delivery Rover:** A ROS 2-enabled heavy tracked chassis (running on a Jetson Nano) navigates the dirt aisles via GPS and 2D LiDAR costmaps. It acts strictly as a mobile transporter and elevator.
* **Mini Cleaning Bot:** Once the delivery rover aligns with the flagged panel coordinate, it deploys a lightweight, specialized panel-crawler directly onto the glass to execute the physical cleaning cycle.

### 4. Closed-Loop Machine Learning Validation

Following the ground intervention, the scout drone executes a secondary verification scan over the same coordinates.

* If the post-cleaning thermal metric matches the ideal Digital Twin baseline, the initial anomaly is mathematically labeled as **surface soiling**.
* If the severe thermal delta persists, the anomaly is labeled as a **permanent physical defect** (e.g., silicon micro-crack).
This continuous compilation of pre- and post-cleaning metrics automatically retrains the primary computer vision weights using absolute physical ground truth, entirely eliminating manual human data annotation.

## Technical Stack

**Software & Middleware**

* **Robotics Framework:** ROS 2 (Humble)
* **Messaging Protocol:** Local Mosquitto MQTT Broker
* **Simulation & Kinematics:** SolidWorks (CAD/Motion), Gazebo (Physics/Navigation)
* **Computer Vision & ML:** OpenCV, PyTorch
* **Edge Compute:** Python

**Hardware Modules (Prototype)**

* **Drone Controller:** Pixhawk 2.4.8 (ArduPilot)
* **Companion Computers:** Raspberry Pi Zero 2 W (Aerial), NVIDIA Jetson Nano / Pi 4 (Ground)
* **Low-Level Control:** ESP32-S3 (Motor PWM, Encoder Interrupts, Actuation)
* **Sensors:** MLX90640 (Thermal), Slamtec RPLIDAR S3 (2D Mapping), H-RTK F9P GNSS, GY-91 IMU

## Current Development Status

The project is currently in the mechanical design and simulation phase, with development focused on the **Delivery Rover**.

* **Active Sprint:** End-to-end CAD modeling of the tracked delivery rover in SolidWorks.
* **Next Steps:** Exporting the delivery rover assembly to URDF for kinematics and navigation testing in Gazebo (ROS 2), followed by the mechanical design of the secondary Mini Cleaning Bot deployment mechanism.