# Spacecraft Dynamics and Control: Mini Project

**Author:** R Sandeepkumar

report.pdf in `\part 2\report` has detailed report (with figures) submitted as part of course project. The readme is a summary of the work done.

## 1. Project Overview
This project focuses on the active attitude stabilization of an unstable, zero momentum biased spacecraft (Astrosat). Because passive stabilization techniques result in poor set-point tracking, this project develops and validates a Proportional-Derivative (PD) control algorithm. The controller is designed using a linearized plant model about an equilibrium condition and is subsequently validated on a non-linear model to evaluate its performance for inertial or earth-pointing mission requirements.

## 2. Mathematical Modeling
### 2.1 Equations of Motion
*   **Kinematics:** The attitude of the spacecraft is formulated using Euler angles (roll $\phi$, pitch $\theta$, and yaw $\psi$), and the corresponding kinematic equations are derived using rotational matrices. 
*   **Dynamics:** The rotational dynamics incorporate the moment of inertia matrix alongside gravity gradient, disturbance, and control torques. Translational dynamics are ignored in this formulation. 
*   **Limitations:** This Euler angle representation is computationally expensive and introduces mathematical singularities.

### 2.2 Linearization
*   Classical control techniques require linear systems, so the 6-state, 3-control Multiple-Input Multiple-Output (MIMO) system was linearized about an equilibrium point.
*   For the design process, the off-diagonal terms of the inertia matrix were assumed to be zero, simplifying the system into three decoupled, second-order differential equations.

## 3. System Analysis
*   **Transfer Function:** The derived transfer functions represent a Type 2 system, which is inherently unstable (characterized by two poles at the origin).
*   **Free Response:** Simulations of both the non-linear and linearized systems without actuator control demonstrated exponential divergence. Eigenvalue analysis of the Jacobian matrix confirmed the instability, showing an eigenvalue in the right-half plane.

## 4. Control System Design
A PD controller was selected to act as a compensator, as adding an integral term would unnecessarily increase the system's type and complexity. A modified PD control law was utilized, taking the derivative of the measurements directly to prevent large slope errors during step changes in the reference signal.

### Gain Tuning Strategy
1.  **Routh Criteria:** Used to establish baseline stability conditions for the gains.
2.  **Performance Requirements:** Preliminary gains were estimated targeting a maximum overshoot ($M_p$) of **10%** and a settling time ($t_s$) of **20 seconds**.
3.  **Root Locus:** Because initial performance targets were only marginally met due to the transient effects of the added zero, the Root Locus method (via MATLAB's `sisotool`) was used to fine-tune the proportional ($K_p$) and derivative ($K_d$) gains.

## 5. Non-Linear Simulation & Results
To ensure the controller's viability on a real-world system, it was tested in a closed-loop feedback simulation against the highly non-linear plant model. 

*   **Attitude Stabilization:** The controller successfully stabilized the satellite to a zero-degree set point. However, non-linear coupling between the states caused the settling time to increase from roughly 9 seconds (in the linear model) to approximately 16 seconds. Furthermore, correcting an error in one axis (e.g., roll) engaged the control torques for the other axes due to these coupled dynamics.
*   **Set Point Tracking:** The system successfully tracked alternating attitude set points when given sufficient time (e.g., switching every 100 seconds). However, when the set points were changed too rapidly (e.g., every 5 seconds), the tracking was poor because the spacecraft dynamics were too slow to respond to the fast reference changes. 

## 6. Limitations & Future Work
The current simulation and control design have the following limitations that outline areas for future improvement:
*   Actuator dynamics and control torque constraints were not taken into consideration.
*   Sensor noise was excluded from the simulation.
*   Position dynamics were assumed to be completely decoupled.
*   The gains were designed for a single, fixed equilibrium condition; implementing gain scheduling would be the logical next step.
