# Robotic Arm Torque Calculation & Servo Motor Selection

1. Introduction

This project calculates the required torque for each joint of a 3-DOF robotic arm with given link lengths and payload. Based on the torque calculations, suitable servo motors are selected. Additionally, it examines whether the selected motors can handle an increased payload of 2 kg using gearboxes, discusses the downsides of gear usage, and proposes alternative solutions.

---
2. Arm Specifications

| Link   | Length (cm) | Length (m) |
| ------ | ----------- | ---------- |
| Link 1 | 15          | 0.15       |
| Link 2 | 10          | 0.10       |
| Link 3 | 4           | 0.04       |

Payload mass: 1 kg (initial), 2 kg (tested)

Gravity (g): 9.81 m/s²

---
3. Torque Calculation Method

Torque at each joint is calculated using the formula:

T = m \times g \times r

Where:

* $m$: mass of the payload (kg)
* $g$: acceleration due to gravity (9.81 m/s²)
* $r$: distance from the joint to the payload (meters)

Distances for each joint are cumulative sums of the link lengths beyond the joint.

---

4. Calculated Torque Values

 For 1 kg Payload:

| Joint   | Distance $r$ (m)          | Torque $T$ (Nm) |
| ------- | ------------------------- | --------------- |
| Joint 3 | 0.04                      | 0.39            |
| Joint 2 | 0.14 (0.10 + 0.04)        | 1.37            |
| Joint 1 | 0.29 (0.15 + 0.10 + 0.04) | 2.85            |

---
5. Selected Servo Motors

| Joint   | Required Torque (Nm) | Selected Servo         | Max Torque (Nm) | Purchase Link                                    |
| ------- | -------------------- | ---------------------- | --------------- | ------------------------------------------------ |
| Joint 1 | 2.85                 | Dynamixel XM430-W350-R | 3.7             | [Buy Here](https://www.robotis.us/xm430-w350-r/) |
| Joint 2 | 1.37                 | Dynamixel XL430-W250-T | 1.4             | [Buy Here](https://www.robotis.us/xl430-w250-t/) |
| Joint 3 | 0.39                 | Dynamixel XL430-W250-T | 1.4             | [Buy Here](https://www.robotis.us/xl430-w250-t/) |

*Note:* XL320 is not sufficient for Joint 3 torque requirements.

---
6. Handling Increased Payload (2 kg)

For 2 kg payload, torque roughly doubles:

| Joint   | New Required Torque (Nm) |
| ------- | ------------------------ |
| Joint 3 | 0.78                     |
| Joint 2 | 2.75                     |
| Joint 1 | 5.69                     |

**Evaluation:**

* Joint 1 motor (3.7 Nm max) **insufficient** — needs gearbox (e.g., 2:1 ratio) or stronger motor.
* Joint 2 motor (1.4 Nm max) borderline — gearbox recommended.
* Joint 3 motor (1.4 Nm max) sufficient.

---
7. Downsides of Using Gearboxes

* Reduced movement speed proportional to gear ratio.
* Increased mechanical complexity and size.
* Backlash leading to precision loss.
* Higher power consumption and noise.

---
 8. Alternatives to Gear Usage

* Use stronger servo motors (e.g., Dynamixel Pro series).
* Reduce link lengths to decrease required torque.
* Add counterweights or springs to balance the load.
* Redesign the arm for lighter payload requirements.

---
9. Torque Calculation Script (Python)

```python
g = 9.81  # gravity acceleration (m/s^2)

link1 = 0.15
link2 = 0.10
link3 = 0.04

def calc_torque(mass):
    T3 = mass * g * link3
    T2 = mass * g * (link2 + link3)
    T1 = mass * g * (link1 + link2 + link3)
    return round(T1, 2), round(T2, 2), round(T3, 2)

torque_1kg = calc_torque(1)
torque_2kg = calc_torque(2)

print("Torque for 1kg payload (Nm):", torque_1kg)
print("Torque for 2kg payload (Nm):", torque_2kg)
```
---
 10. Conclusion

* Torque requirements are calculated accurately.
* Selected servo motors fit 1 kg payload requirements.
* For 2 kg payload, motor upgrade or gearboxes are necessary.
* Gearboxes have downsides; alternative solutions should be considered depending on design priorities.
-
