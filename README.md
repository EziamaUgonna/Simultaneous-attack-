
# Anomaly Modeling in Connected and Automated Vehicles (CAV)

## Overview

This repository contains code and documentation for anomaly modeling in Connected and Automated Vehicles (CAV). The focus is on identifying and simulating sensor anomalies to evaluate the robustness and resilience of sensor fusion systems. The code is based on the SPMD dataset, which includes in-vehicle speed (sensor 1), GPS speed (sensor 2), and in-vehicle acceleration (sensor 3).

## Dataset

The SPMD dataset used for this experiment includes:
- **Sensor 1:** In-vehicle speed (s)
- **Sensor 2:** GPS speed (GPSS)
- **Sensor 3:** In-vehicle acceleration (Ax)

Due to the lack of publicly available datasets featuring anomalous sensor behaviors due to attacks, we injected anomalies into the SPMD datasets through simulations. Our anomalous/attack scenario is almost like a real-time scenario because we used a real-time SPMD dataset from the United States Department of Transportation (USDOT) and the anomalies we introduced followed the distributions of real-time anomaly cases. The anomalies modeled and injected into the data are instant, bias, and gradual drift anomalies.

## Attack Simulation

In our attack simulation experiment, we argued that in real-life scenarios, simultaneous attacks could perturb multiple sensors concurrently, unlike the independent perturbations assumed in previous studies. In this case, malicious actors might intentionally compromise multiple sensor readings simultaneously, introducing spatial anomalies and presenting a more sophisticated threat.

The attack modeling involves the simultaneous perturbation of sensor readings (s, GPSS, and Ax) for 10-time steps when certain conditions are met. This perturbation introduces spatial anomalies, challenging the robustness and resilience of sensor fusion systems.




<br>Preferred Citation:

