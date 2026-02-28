# Engineering Build Log: Sim-to-Real Control Systems

## 1. The Core Objective (The "Why")
To establish a rigorously verified pipeline wherein control logic operates with exact parity on both NVIDIA Isaac Sim digital twins and physical robot hardware. The objective is to absolutely eliminate the "reality gap," guaranteeing that heuristic models trained in simulation immediately transfer to the edge with zero behavioral drift.

## 2. The Toolchain & Constraints
*   **Simulation Engine:** NVIDIA Isaac Sim
*   **Middleware Integration:** ROS 2 OmniGraph Bridge
*   **Control Logic:** Python / C++ 
*   **Hard Constraints:** Absolute clock synchronization between the deterministic PhysX engine and the asynchronous ROS 2 middleware (`use_sim_time = true`).

## 3. The Execution Sequence
1.  **Simulation Instantiation:** Configured NVIDIA Isaac Sim with accurate rigid-body asset properties.
2.  **Data Graph Mapping:** Constructed Isaac Sim OmniGraph Action Graphs to deterministically map physical joint states to ROS 2 `sensor_msgs/JointState` topics.
3.  **Control Architecture:** Engineered the ROS 2 node architecture (Python and C++) to publish velocity command messages to the simulated articulation matrices.
4.  **Reality Verification:** Executed closed-loop verification protocols to ensure commanded states matched the simulated PhysX states with bounded latency.

## 4. Architectural Decisions
Constructing manual Python bridges rapidly introduces serialization overhead and latency spikes. We utilized **NVIDIA OmniGraph** as the direct action translation layer. OmniGraph operates closer to the core C++ physics engine, radically reducing latency and explicitly coupling the PhysX step time with the ROS 2 transform broadcasts. This ensures visual and physical telemetry remains synchronous.

## 5. The Wavefront
*   **Phase 1:** Integrate deep Reinforcement Learning (RL) ONNX models directly into the ROS 2 node architecture for edge inference.
*   **Phase 2:** Introduce programmatic Isaac Sim Domain Randomization (varying mass, friction, lighting) to force control policies to generalize against chaos.
*   **Phase 3 (L5 Trajectory):** Implement continuous real-world telemetry ingestion directly back into the OmniGraph digital twin, creating an aggressively updating Bayesian loop that refines the simulated physics engine in real-time.
