# Virtual EV Challenge 2025 — IITG Racing

## 📌 Project Overview
This repository contains the simulation models, AI agent logic, and middleware integrations developed for the Virtual EV Challenge conducted by IPG Automotive and Bosch. 

**The Problem:** Electric vehicles navigating long routes face the dual challenge of managing energy efficiency on the fly and making optimal charging stop decisions to avoid battery depletion. The objective of this project was to dynamically optimise an EV's speed and charging schedule to minimise total trip time while ensuring safe battery state-of-charge (SoC) thresholds over a 153 km route.

---

## 🚀 Approach & Architecture

### Phase 1: Optimal Speed and Motor Force Modelling (Simulink)
The first phase focused on baseline vehicle powertrain modeling to maximize energy efficiency. 
* **Model Generation:** Built a vehicle and powertrain model using MATLAB Simulink, which was exported as a Functional Mock-up Unit (FMU) compliant with FMI 2.0 for Co-Simulation.
* **Dynamic Calculations:** The model calculates the optimal vehicle speed (`V_opt`) and required motor force (`F_motor`) based on environmental and physical parameters (e.g., air density of 1.225 kg/m³, rolling resistance, and aerodynamic drag).
* **Constraints:** The optimal speed is mathematically constrained to never exceed the maximum vehicle speed limit of 16.667 m/s.

### Phase 2: AI-Integrated Charging Optimisation
The second phase introduced real-time, AI-driven decision-making to handle charging logistics during the route. 
* **System Components:** The architecture couples CarMaker (vehicle dynamics simulator), KUKSA Databroker (VSS middleware), and a multi-agent AI system (using `uagents`). 
* **Agent Logic:** * A **Requester Agent** continuously monitors the vehicle's live SoC. If the SoC drops below 25%, it triggers the Optimizer Agent.
  * The **Optimizer Agent** evaluates available candidate stations based on travel time, ensuring the predicted SoC upon arrival is ≥ 15%, and verifying the final destination SoC will remain ≥ 20%.
* **Data Bridge:** A PyCarMaker bridge facilitates real-time, bidirectional signal translation between CarMaker and KUKSA.

---

## 📊 Quantifiable Outcomes

Our integrated approach yielded the following validated simulation results:

* **Baseline Route Performance:** Successfully completed a 153 km continuous drive cycle in 2.67 hours (9,612.2 s) with a residual SoC of 19% prior to AI charging integration.
* **System Latency:** Achieved an end-to-end processing latency of < 200 ms from CarMaker state publication to the AI optimizer's decision write-back.
* **Decision Reliability:** Maintained 100% decision correctness (fraction of feasible optimizer decisions) with zero crashes or deadlocks across extended scenarios.
* **Accuracy:** Kept the SoC estimation error highly accurate, achieving a Mean Absolute Error (MAE) of < 2–5 percentage points.
* **Validation:** Successfully validated the AI charging orchestration across three distinct initial SoC scenarios: 50%, 40%, and 30%.

---

## 🛠️ Tech Stack
* **Simulation:** MATLAB Simulink, IPG CarMaker
* **Middleware:** KUKSA Databroker (Vehicle Signal Specification - VSS)
* **AI & Integration:** Python, `uagents`, PyCarMaker
