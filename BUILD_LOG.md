# Engineering Build Log: Sim-to-Real Control Systems

## 1. The Core Objective (The "Why")
To establish a mathematically verified pipeline where control logic written for a digital twin functions identically on physical hardware. This eliminates the "reality gap" and allows for rapid, destructive testing of control policies in simulation before risking irreversible damage to physical assets. It validates polyglot proficiency (Python/C++) and the critical Isaac Sim-to-ROS 2 bridging.

## 2. The Toolchain & Constraints
* **Simulation Engine:** NVIDIA Isaac Sim
* **Middleware:** ROS 2 (Humble) + NVIDIA OmniGraph
* **Languages:** Python (Rapid Prototyping) & C++ (High-Performance Nodes)
* **The Hard Constraint:** Maintaining exact clock synchronization between the Isaac Sim physics stepper and the ROS 2 network clock to prevent temporal aliasing in control signals (`use_sim_time = true`).

## 3. The Execution Sequence
* **Step 1: ROS 2 Foundation:** Built standalone publisher/subscriber nodes in both Python (rapid prototyping) and C++ (high-performance deployment) using standard Colcon builds.
* **Step 2: Isaac Sim Scripting:** Created an advanced standalone Python script (`franka_wave.py`) leveraging the Isaac Sim API to interact directly with the robot articulated joints.
* **Step 3: The OmniGraph Bridge:** Constructed the visual OmniGraph Action Graph to map Isaac Sim's native physical states deterministically to standard ROS 2 `sensor_msgs/JointState` topics. 
* **Step 4: The Reality Verification:** Executed a closed-loop control test by launching the dual-node ecosystem, proving the external logic seamlessly interfaces with the simulated hardware exactly as it would with physical actuators.

## 4. Architectural Decisions & Trade-offs
* **Why use OmniGraph instead of writing custom ROS 2 bridges?** OmniGraph operates directly within the Omniverse execution loop. It reduces latency and provides a deterministic, visual MBSE-style mapping of how data flows from the PhysX engine into the ROS middleware.
* **Why multi-language (Python & C++)?** Python is ideal for AI-layer rapid iteration and data visualization, while C++ is essential for deterministic, real-time control loops. Maintaining pure parity across both validates production-readiness.

## 5. The Wavefront (Next Steps)
* **Project Alpha:** Implement a Reinforcement Learning (RL) policy trained in Isaac Sim and deploy the resulting ONNX model directly to the ROS 2 control node.
* **Project Beta:** Introduce domain randomization (varying friction, mass, and sensor noise) in Isaac Sim to force the control system to become highly robust before physical deployment.
