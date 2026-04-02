# Lab 5 of SC2104  — Self-Levelling Platform


## Demo Video
https://github.com/user-attachments/assets/f28ebde6-beee-4287-b6c0-6c971c0d67db

## What's going on ?

The STM32F4 reads tilt angles from an IMU ( Accelerometer + Gyroscope ), and uses a PID controller to drive two servo motors to keep a platform level. When you tilt the platform, the PID corrects the servos back to target. A lil closed-loop feedback control system.

Data flows in a loop every iteration:
1. **IMU** reads raw accelerometer + gyroscope data (I2C from MPU6050)
2. **Kalman Filter** fuses both sensors → clean `roll_KF`, `pitch_KF`
3. **Moving Average (10-tap)** smooths further → `roll_angle`, `pitch_angle`
4. **PID** ID computes correction from error = target - angle — proportional to `Kp`, `Kd` , `Ki` 
5. **Servo** receives `nominal_PWM ± PID_correction` as CCR value
6. Platform tilts → IMU reads new angle → loop repeats

---

## Quick Setup

### Hardware
- STM32F4 board mounted on the self-levelling platform
- MPU6050 IMU (communicates via **I2C**)
- 2× Servo motors connected to **TIM12 CH1** (roll) and **TIM12 CH2** (pitch)

### STM32CubeIDE Config
| Peripheral | Setting |
|---|---|
| I2C2 | For MPU6050 |
| TIM12 CH1 & CH2 | PWM Generation, 50 Hz (20ms period) |
| TIM6 | Microsecond delay timer |
| USART3 | SerialPlot output (115200 baud) |














---




