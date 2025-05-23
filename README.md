# Optimal Hardware Implementation of Multi-Level Cache Based on Software Simulation
**Project Goal:** To analyze the optimal miss rates of various multi-level cache configurations through software simulation and then design a hardware implementation of these configurations to assess their impact on power consumption, timing, and utilization on a benchmark FPGA. The main objective is to determine the hardware costs and drawbacks associated with high-performance multi-level cache designs.

**Problem Statement:**
Traditional cache performance metrics (like read/write misses, miss ratio, writebacks) do not fully account for other crucial design impacts such as hardware utilization and power consumption. A comprehensive analysis is needed to balance these classical metrics with hardware-centric considerations to design cache architectures better suited for specific processor applications.

**Approach & Methodology:**
The project was divided into two main branches: a software-based cache simulator and a hardware implementation.

**Software Simulation:**
- A modular cache simulator was developed in Java, capable of accepting various cache parameters including block size, L1/L2 size, L1/L2 associativity, replacement policy (FIFO, LRU, LIFO, MRU), inclusion policy (inclusive, non-inclusive), and trace files.
Experiments were conducted by varying replacement policies (FIFO vs. LRU), cache sizes (1KB/4KB vs. 4KB/16KB for L1/L2), and associativity levels (4-way/8-way vs. 2-way/16-way for L1/L2).
- Trace files (compress, go, gcc, perl, vortex) each with 100,000 addresses and operations, were used to test the simulator.
Hardware Implementation:
- The cache design was implemented using SystemVerilog, a Register Transfer Level (RTL) language, leveraging its features like floating-point data types for accurate hit ratio calculations.
- Vivado 2022.1, an IDE supporting Xilinx FPGAs, was used for creating, executing, and synthesizing the RTL code. Vivado allowed for waveform simulations and estimations of hardware power usage and physical space on the chip.
The multi-level cache architecture comprised four main modules: cache_top (top-level module for instantiating the engine and managing I/O), cache_tb (test bench), cache_params (for defining cache parameters), and cache_engine (core functionality for different cache configurations).
- A Finite State Machine (FSM) with seven states (IDLE, READ, SEARCH, SHIFTFULL, SHIFTEMPTY, CACHEHIT, DONE) was used to construct the multi-level cache functionality, managing operations like address processing, cache searching, and hit/miss handling.
- Hardware tests measured utilization and power consumption for different replacement policies (LRU vs. FIFO), cache sizes (1KB/4KB vs. 4KB/16KB), and associativity levels (4-way/8-way vs. 2-way/16-way) on a benchmark FPGA (XC7A200T-1FF1156C).

**Key Contributions & Results:**

Software Simulation Insights:
- Demonstrated that LRU replacement policy generally resulted in better miss rates compared to FIFO, although the improvement was slight on average.
- Confirmed that increasing L1 cache size consistently reduced its miss ratio across all trace files, providing performance improvements up to 3.52x.
- Observed inconsistent L2 cache performance with increased size, suggesting other factors (like memory access patterns) influence its miss ratio.
- Showed that higher L1 associativity generally led to lower miss rates, while L2 associativity showed similar trends, with exceptions for the 'compress' trace.

**Hardware Implementation Insights:**
- Successfully implemented a multi-level cache design in SystemVerilog, including configurable inclusion, replacement, and write policies.
- Utilized Vivado for hardware synthesis and analysis, enabling estimation of power usage and on-chip space utilization.
- Found that LRU replacement policy, while offering better hit rates, significantly increased LUT (Look-Up Table) usage (twice as many as FIFO) and power consumption (4% increase).
- Demonstrated that increasing cache size (e.g., from 1KB L1/4KB L2 to 4KB L1/16KB L2) roughly quadrupled LUT usage and significantly increased power consumption (from 1.26W to 2.488W), indicating a direct correlation between cache size and hardware cost.
- Observed that increasing associativity generally improved miss rates but led to diminishing returns and increased power/thermals if increased too much.

**Challenges & Limitations:**
- Hardware Trade-offs: The project highlighted the inherent trade-offs between cache performance (miss rates) and hardware costs (power consumption, utilization, timing).
- Optimal Configuration Complexity: Determining an truly "optimal" cache configuration requires balancing numerous interdependent parameters and their impact on both software performance and hardware resources.

**Conclusion:**
This project successfully analyzed the complex interplay between multi-level cache configurations and their impact on both software performance (miss rates) and hardware metrics (power, utilization). The findings provide valuable insights into the trade-offs involved in designing high-performance cache systems, emphasizing that while larger caches and more sophisticated policies (like LRU) can improve miss rates, they come with significant increases in hardware cost and power consumption. The work serves as a foundational analysis for future cache architecture design, underscoring the importance of considering all performance metrics for application-specific optimization.
