# [cite_start]Virtual EV Challenge 2025 — IITG Racing [cite: 1, 2, 105]

## 📌 Project Overview
[cite_start]This repository contains the simulation models, AI agent logic, and middleware integrations developed for the Virtual EV Challenge conducted by IPG Automotive and Bosch[cite: 2, 4]. 

**The Problem:** Electric vehicles navigating long routes face the dual challenge of managing energy efficiency on the fly and making optimal charging stop decisions to avoid battery depletion. [cite_start]The objective of this project was to dynamically optimise an EV's speed and charging schedule to minimise total trip time while ensuring safe battery state-of-charge (SoC) thresholds over a 153 km route[cite: 28, 51, 52].

---

## 🚀 Approach & Architecture

### [cite_start]Phase 1: Optimal Speed and Motor Force Modelling (Simulink) [cite: 15, 77]
[cite_start]The first phase focused on baseline vehicle powertrain modeling to maximize energy efficiency[cite: 78, 79]. 
* [cite_start]**Model Generation:** Built a vehicle and powertrain model using MATLAB Simulink, which was exported as a Functional Mock-up Unit (FMU) compliant with FMI 2.0 for Co-Simulation[cite: 12, 78].
* [cite_start]**Dynamic Calculations:** The model calculates the optimal vehicle speed (`V_opt`) and required motor force (`F_motor`) based on environmental and physical parameters (e.g., air density of 1.225 kg/m³, rolling resistance, and aerodynamic drag)[cite: 14, 17, 21].
* [cite_start]**Constraints:** The optimal speed is mathematically constrained to never exceed the maximum vehicle speed limit of 16.667 m/s[cite: 19].

### [cite_start]Phase 2: AI-Integrated Charging Optimisation [cite: 31, 81]
[cite_start]The second phase introduced real-time, AI-driven decision-making to handle charging logistics during the route[cite: 37, 84]. 
* [cite_start]**System Components:** The architecture couples CarMaker (vehicle dynamics simulator), KUKSA Databroker (VSS middleware), and a multi-agent AI system (using `uagents`)[cite: 35, 36, 37]. 
* **Agent Logic:** * A **Requester Agent** continuously monitors the vehicle's live SoC. [cite_start]If the SoC drops below 25%, it triggers the Optimizer Agent[cite: 58, 59].
  * [cite_start]The **Optimizer Agent** evaluates available candidate stations based on travel time, ensuring the predicted SoC upon arrival is ≥ 15%, and verifying the final destination SoC will remain ≥ 20%[cite: 60, 61, 62, 63, 64].
* [cite_start]**Data Bridge:** A PyCarMaker bridge facilitates real-time, bidirectional signal translation between CarMaker and KUKSA[cite: 69, 71].

---

## 📊 Quantifiable Outcomes

Our integrated approach yielded the following validated simulation results:

* [cite_start]**Baseline Route Performance:** Successfully completed a 153 km continuous drive cycle in 2.67 hours (9,612.2 s) with a residual SoC of 19% prior to AI charging integration[cite: 28].
* [cite_start]**System Latency:** Achieved an end-to-end processing latency of < 200 ms from CarMaker state publication to the AI optimizer's decision write-back[cite: 75].
* [cite_start]**Decision Reliability:** Maintained 100% decision correctness (fraction of feasible optimizer decisions) with zero crashes or deadlocks across extended scenarios[cite: 75].
* [cite_start]**Accuracy:** Kept the SoC estimation error highly accurate, achieving a Mean Absolute Error (MAE) of < 2–5 percentage points[cite: 75].
* [cite_start]**Validation:** Successfully validated the AI charging orchestration across three distinct initial SoC scenarios: 50%, 40%, and 30%[cite: 91].

---

## 🛠️ Tech Stack
* [cite_start]**Simulation:** MATLAB Simulink, IPG CarMaker [cite: 12, 35]
* [cite_start]**Middleware:** KUKSA Databroker (Vehicle Signal Specification - VSS) [cite: 36]
* [cite_start]**AI & Integration:** Python, `uagents`, PyCarMaker [cite: 37, 44]
