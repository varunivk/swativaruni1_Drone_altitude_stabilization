# 🚁 Drone Altitude Stabilization using PID Control

## 📌 Project Overview

This project implements a PID-based closed-loop control system for maintaining drone altitude under wind disturbance using MATLAB and Simulink.

The controller continuously monitors altitude error and adjusts thrust to stabilize the drone even under external disturbances.

---

# 🎯 Objectives

- Maintain stable drone altitude
- Minimize overshoot
- Reduce settling time
- Achieve near-zero steady-state error
- Reject wind disturbance effectively

---

# 🛠 Software Used

- MATLAB
- Simulink
- Control System Toolbox

---

# ⚙ Transfer Function

G(s) = 1 / (s² + 2s + 5)

---

# 🧠 PID Controller

The PID controller used is:

C(s) = Kp + Ki/s + Kd*s

## PID Parameters

| Parameter | Value |
|---|---|
| Kp | 42 |
| Ki | 24 |
| Kd | 18 |

---

# 🧩 Features

✅ Closed Loop Feedback Control  
✅ PID Controller Implementation  
✅ Wind Disturbance Rejection  
✅ Automatic Simulink Generation using MATLAB Script  
✅ Root Locus Analysis  
✅ Bode Plot Analysis  
✅ Stability Verification  
✅ Performance Metrics Analysis  
✅ PID Tuning Visualization Animation

---
---

# 🎥 PID Tuning Visualization

The following animation demonstrates how the drone altitude response changes when PID parameters are varied.

- Increase in Kp improves response speed
- Ki reduces steady-state error
- Kd improves damping and stability

---

# 🚀 Working Principle

1. The desired altitude is provided through a Step Input block.
2. The feedback system continuously compares desired altitude with actual altitude.
3. The PID controller calculates the error and adjusts the thrust accordingly.
4. Wind disturbance is introduced during simulation.
5. The controller automatically compensates for disturbance and stabilizes the drone altitude.

---

# 📂 Project Files

| File Name | Description |
|---|---|
| create_drone_simulink.m | Automatically generates Simulink model |
| drone_control.m | MATLAB analysis and graphs |
| Drone_Altitude_Control.slx | Simulink project file |
| PID_Tuning_Visualization.gif | PID tuning animation |
| video_demo.mp4 | Project demonstration video |

---

# 📋 Results

The system successfully:

- Stabilizes drone altitude
- Minimizes overshoot
- Reduces settling time
- Rejects wind disturbance
- Achieves near-zero steady-state error

---

# 🔮 Future Improvements

- Adaptive PID tuning
- Real-time sensor integration
- Hardware implementation
- Autonomous navigation system

---

# 👨‍💻 Team Members

- Varuni V K
- Swati S S

---

# ✅ Conclusion

A PID-based closed-loop control system was successfully designed and implemented for drone altitude stabilization using MATLAB and Simulink. The system effectively maintains stable altitude under external wind disturbance while ensuring good transient and steady-state performance.
