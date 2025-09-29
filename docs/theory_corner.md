# **Theory corner**

!!! Note annotate "" 

## Impedance Control

**Concept**:
- Impedance control is like controlling how "stiff" or "flexible" the robot feels when it interacts with the environment. It defines how the robot resists motion in response to external forces.
Impedance control regulates the force output of a system based on the position error.

**Analogy**:
- Imagine the robot's end effector is connected to the environment by a virtual spring-damper system. The robot adjusts the force it applies to mimic a specific stiffness, damping, and inertia.

**Implementation**:
- The robot measures its position, velocity, and possibly the force it applies.
- The controller calculates the desired force or torque output based on the difference between the actual and desired position/velocity, considering virtual mass, damping, and stiffness.
- The robot then applies this calculated force/torque to maintain the desired interaction characteristics.

**Example**:
- A robot arm is drilling into a surface. Impedance control ensures the robot maintains a steady drilling force despite variations in the surface hardness. If the surface becomes harder, the robot increases its force to maintain the drilling motion.

**Equation**:

𝐹_control = 𝑀𝑥̈ + 𝐵𝑥̇ + 𝐾𝑥

- 𝑀 – virtual mass  
- 𝐵 – **virtual damping coefficient**  
- 𝐾 – **virtual stiffness**  
- 𝐹_ext – external force measured by a force/torque sensor  
- 𝑥 – position  
- 𝑥̇ – velocity  
- 𝑥̈ – acceleration

---

!!! Note annotate "" 

### Admittance Control

**Concept**:
- Admittance control is like controlling how easily the robot moves in response to external forces. It defines how the robot adapts its position or velocity based on the forces applied by the environment.
Admittance control regulates the position or velocity of a system based on the force error.

**Analogy**:
- Imagine the robot's end effector is like a balloon tethered to a point. The robot adjusts its position or velocity to move compliantly with the applied forces, mimicking a specific level of compliance and damping.

**Implementation**:
- The robot measures the external force applied to it and its own position and velocity.
- The controller calculates the desired position or velocity based on the measured force, considering virtual compliance and damping.
- The robot then adjusts its motion to match the desired position or velocity.

**Example**:
- A robot arm is holding a tray while a person places objects on it. Admittance control allows the robot to adjust its position smoothly to accommodate the added weight, ensuring the tray remains stable.

**Equation** (for velocity admittance control):

In many practical controllers, the virtual inertia 𝑀 is neglected to reduce complexity.  
The model simplifies to:

𝐹_ext = 𝐵𝑥̇ + 𝐾𝑥

If we solve for the desired velocity 𝑣_desired, we get:

𝑣_desired = (𝐹_ext − 𝐾𝑥) / 𝐵

✅ **Meaning of terms:**  
- 𝐵 – virtual damping coefficient, controls how *fast* the robot reacts  
- 𝐾 – virtual stiffness, determines how much position error contributes to motion  
- 𝐹_ext – external force input  
- 𝑣_desired – commanded velocity  

**Intuition:**  
- Increasing 𝐵 → slower, more damped motion  
- Increasing 𝐾 → stiffer response (less displacement for a given force)

**Equation** (for position admittance control):

You can also solve for the desired position 𝑥_desired directly:

𝑥_desired = (1/𝐾) · 𝐹_ext − (𝐵/𝐾) · 𝑣

✅ **Meaning of terms:**  
- (1 / 𝐾) · 𝐹_ext – position offset due to external force (compliance)  
- (𝐵 / 𝐾) · 𝑣 – damping correction (smooths motion and reduces overshoot)  
- 𝐵 / 𝐾 – ratio defining how strongly damping influences position response

---

!!! Note annotate "" 

### Summary

**Impedance Control**:
- **Robot's Role**: The robot reacts to position/velocity changes by applying forces to resist motion.
- **Control Focus**: Force/torque output based on position/velocity error.
- **Interaction Feel**: Defines how stiff or flexible the robot feels (resistance).

**Admittance Control**:
- **Robot's Role**: The robot adjusts its motion in response to external forces.
- **Control Focus**: Position/velocity based on external force input.
- **Interaction Feel**: Defines how easily the robot moves (compliance).

---

### Choosing the Control Method

- **Impedance Control**: Suitable for tasks requiring precise force regulation and maintaining a specific motion despite external disturbances, such as machining or tasks where consistent force application is critical.
- **Admittance Control**: Suitable for tasks requiring adaptive and compliant motion, especially when interacting with unpredictable environments or humans, such as assisting or collaborating with humans in handling delicate objects.

By understanding these control strategies in terms of how the robot interacts with its environment, one can better appreciate their applications and benefits in various robotic tasks.

---

!!! Note annotate "" 

## Compliance

Compliance, in the context of control systems and robotics, refers to the ability of a system to yield or deform in response to an applied force. It is the inverse of stiffness (or rigidity), which measures the resistance of a system to deformation. High compliance means the system is easily deformable or flexible, whereas low compliance (high stiffness) means the system is resistant to deformation.

### Key Concepts of Compliance

- **Compliance** (𝐶): It is defined as the inverse of stiffness (𝐾). Mathematically:

𝐶 = 1 / 𝐾

- **Stiffness** (𝐾): It quantifies the resistance of a system to deformation. High stiffness means the system resists deformation strongly when a force is applied.

### Compliance in Control Systems

In control systems, particularly in robotics, compliance is crucial for safe and effective interaction with the environment, especially in tasks that involve human-robot interaction or handling delicate objects.

### Example: Compliance in a Robotic Arm

Imagine a robotic arm equipped with compliance control. Here’s how compliance plays a role:

1. **Interaction with the Environment**:
   - When the robotic arm encounters an obstacle, a compliant control system allows the arm to yield slightly, reducing the risk of damage to both the arm and the obstacle.
   - High compliance makes the arm flexible, allowing it to absorb and adapt to external forces smoothly.

2. **Human-Robot Interaction**:
   - In collaborative tasks where a human and a robot work together, compliance ensures that the robot can adapt to the human’s movements without exerting excessive force, enhancing safety and cooperation.
   - If the human applies a force to the robot, a compliant robot will move or adjust its position in response, preventing injury or discomfort.

### Practical Implementation

Compliance can be implemented in robotic systems through both passive and active means:

- **Passive Compliance**:
  - Achieved using physical elements like springs, dampers, and flexible materials.
  - Example: A robotic gripper with soft, flexible fingers that conform to the shape of the object being held.

- **Active Compliance**:
  - Achieved using control algorithms that adjust the robot's motion or force output in real-time based on sensor feedback.
  - Example: A robotic arm that uses force sensors to detect applied forces and adjusts its position or velocity accordingly, implementing virtual compliance through software.

### Compliance in Control Equations

In control equations, compliance is often reflected in how the system responds to forces. For example, in admittance control, the system's compliance is an integral part of the control law:

𝑥_desired = 𝐶 · 𝐹_ext

Where:
- 𝑥_desired is the desired position.
- 𝐶 is the compliance (inverse of stiffness).
- 𝐹_ext is the external force applied to the system.

This equation shows that the desired position change is directly proportional to the applied force, with the proportionality factor being the compliance.

---

!!! Note annotate "" 

## Summary

Compliance is a measure of how easily a system deforms in response to an applied force. In robotics and control systems, compliance is crucial for enabling smooth, adaptive, and safe interactions with the environment and humans. It is the inverse of stiffness and can be implemented both passively and actively. Understanding compliance helps in designing control systems that can effectively manage forces and movements, ensuring both precision and safety in robotic operations.

!!! Note annotate "" 
