# Cortex-ARM33-parts-of-CPU-2250-resister
Cortex ARM33 

Core Registers (R0 to R3) — Arguments and Result Pipeline

[Register Identification: R0, R1, R2, R3]
[Type: General Purpose / Scratch Registers]
[Bit Width: 32-bit each]

FUNCTIONAL ARCHITECTURE:
These four registers form the primary entry and exit pipeline for mathematical computations. In deep-space simulation models, they bypass the system RAM entirely to process real-time vector transitions.

SIMULATION ALLOCATION MATRIX:
- R0 / R1: Paired together to form a single 64-bit input slot for Double-Precision Floating-Point coordinates (e.g., Satellite X-axis orbital position).
- R2 / R3: Paired together to form the secondary 64-bit input slot for velocity vectors (e.g., Velocity X-axis components).

EXECUTION DYNAMICS:
When calling the RP2350’s optimized Boot ROM double-precision math acceleration routines, the CPU expects the mathematical operands to be directly pre-loaded into these specific registers. Upon execution completion, the hardware overwrites R0 and R1 to return the definitive 64-bit calculation output in a single CPU cycle.

Low Registers (R4 to R7) — Spacecraft Core State Vectors

[Register Identification: R4, R5, R6, R7]
[Type: General Purpose / Callee-Saved Registers]
[Bit Width: 32-bit each]

FUNCTIONAL ARCHITECTURE:
These registers are highly efficient because they are accessible by all Thumb-2 instruction variants. In an active simulation engine, these are designated as static storage zones to hold critical parameters across iterative loops without triggering Stack operations.

SIMULATION ALLOCATION MATRIX:
- R4: Retains the structural mass properties of the spacecraft or the primary gravitational parameter (μ) of the central celestial body.
- R5: Dedicated to tracking the real-time simulation delta-time step (dt) down to nanosecond precision.
- R6: Holds the orbital eccentricity vector data.
- R7: Preserves the immediate atmospheric drag coefficients or solar radiation pressure variables.

EXECUTION DYNAMICS:
Because these registers are preserved across subroutines, the simulation engine can branch into nested geometric calculations and return without losing the core physics environment baseline.

High Registers (R8 to R11) — Attitude Dynamics Matrix
[Register Identification: R8, R9, R10, R11]
[Type: General Purpose Registers]
[Bit Width: 32-bit each]

FUNCTIONAL ARCHITECTURE:
Operates within the high register bank of the Cortex-M33 core. These registers are utilized to process multi-dimensional arrays, such as quaternions or transformation matrices, required for attitude determination and control systems (ADCS).

SIMULATION ALLOCATION MATRIX:
- R8: Dedicated to the Pitch angular velocity component.
- R9: Dedicated to the Roll angular velocity component.
- R10: Dedicated to the Yaw angular velocity component.
- R11: Reserved as a fast scalar multiplier for real-time thruster vector alignment corrections.

EXECUTION DYNAMICS:
These registers interact directly with the processor’s inline Barrel Shifter. This allows data to be bit-shifted (scaled) instantly during an addition or subtraction instruction, removing the need for a separate scaling clock cycle.


Intra-Procedure Call Scratch Register (R12) — Dynamic Math Scratchpad

[Register Identification: R12]
[Type: Scratch Register / IP Register]
[Bit Width: 32-bit]

FUNCTIONAL ARCHITECTURE:
Functioning as the Intra-Procedure call scratch register, R12 acts as a high-speed, temporary computational buffer inside the CPU core.

SIMULATION ALLOCATION MATRIX:
- R12: Acts as the dedicated mathematical scratchpad for calculating immediate vector differentials, cross-products, and temporary intermediate values during Runge-Kutta numerical integrations.

EXECUTION DYNAMICS:
The simulation pipeline uses R12 to store short-lived intermediate results during matrix transformations. This eliminates the latency of reading and writing to the L1 cache or SRAM, maximizing execution determinism.

Stack Pointer (R13 / SP) — Memory Frontier Partitioning

[Register Identification: R13 / SP (MSP & PSP)]
[Type: Stack Pointer]
[Bit Width: 32-bit]

FUNCTIONAL ARCHITECTURE:
Manages the memory boundary lines. It contains two physically distinct registers: the Main Stack Pointer (MSP) used for system exceptions and the Process Stack Pointer (PSP) used for application execution.

SIMULATION ALLOCATION MATRIX:
- MSP: Points to an isolated, secure memory block dedicated exclusively to handling critical system faults (e.g., numerical division-by-zero errors or orbital trajectory overflow faults).
- PSP: Points to the main simulation routine thread, protecting it from external system calls.

EXECUTION DYNAMICS:
By explicitly partitioning the MSP and PSP at the register level, any sudden calculation anomaly within the complex simulation physics model is contained, preventing system-wide stack corruption.

Link Register (R14 / LR) — Zero-Overhead Branch Routing

[Register Identification: R14 / LR]
[Type: Link Register]
[Bit Width: 32-bit]

FUNCTIONAL ARCHITECTURE:
Stores the return address for subroutines, function calls, and exceptions. It enables the execution of non-blocking, nested mathematical operations.

SIMULATION ALLOCATION MATRIX:
- R14: Temporarily captures the return coordinates when jumping between the raw physics loop and the spacecraft's internal Guidance, Navigation, and Control (GNC) algorithms.

EXECUTION DYNAMICS:
By holding the return address directly inside R14, the CPU avoids pushing the program state onto the RAM stack. This creates a zero-overhead branch structure, allowing the simulation to loop through thousands of orbital iterations with zero memory latency.

Program Counter (R15 / PC) — Execution Vector Engine

[Register Identification: R15 / PC]
[Type: Program Counter]
[Bit Width: 32-bit]

FUNCTIONAL ARCHITECTURE:
Points directly to the memory address of the instruction currently being executed by the pipeline. 

SIMULATION ALLOCATION MATRIX:
- R15: Controls the precise pacing of the entire simulation pipeline. It cycles sequentially through the compiled orbital mechanics instructions stored in the RP2350’s high-speed execute-in-place (XIP) cache or tightly-coupled memory.

EXECUTION DYNAMICS:
Manually or structurally intercepting the value of R15 allows the simulation to instantly jump to specific error-recovery routines or alternative orbital trajectory branches if a simulated collision or structural failure condition is reached.

