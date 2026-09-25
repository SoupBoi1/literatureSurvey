
# Introduction

- To reach their full potential, BISNs must support complete sense–compute–communicate pipeline
	- communication or actuation must complete before application data becomes stale
- Meeting these requirements is challenging because execution depends on harvested-energy dynamics, storage capacity, and power-delivery-network (PDN) behavior.

- However, eventual progress is insufficient for deadline-constrained pipelines. 
	- A task that resumes correctly may still finish too late. 
- HAIL therefore focuses on a complementary problem, i.e., selecting and scheduling compute substrates so that each pipeline stage can complete within its timing constraint under the current energy context.


- This paper presents HAIL, a heterogeneous batteryless architecture and runtime framework that addresses these challenges by combining iso-ISA cores with complementary power-performance characteristics and energy-aware scheduling.
- HAIL targets dependable execution of time-critical full-pipeline workloads while exploiting favorable harvesting intervals for opportunistic execution.


## Contribution 
 - HAIL, a heterogeneous batteryless system architecture that 
	 - leverages multiple iso-ISA RISC-V cores and 
	 - operating points across distinct power-performance regions, 
		 - enabling dependable timely execution of time-critical full-pipeline tasks 
			 - while still enabling efficient opportunistic use during periods of favorable energy- harvesting.
 - At design time, We propose a static core-selection methodology that uses 
		 - anticipated harvested-energy estimates, 
		 - application timing deadlines, 
		 - and area limits
	 - to identify a set of iso-ISA cores 
	 - for a custom batteryless SoC that prioritizes pipeline-deadline dependability while maintaining the throughput of opportunistic compute tasks.
 - HAIL further introduces an energy-aware runtime scheduler 
	 - that dispatches each time-critical task to the most energy-efficient feasible core and operating point under the current energy context.
	
	 - Using an RF-trace-driven keyword-spotting workload, 
	 - we demonstrate in simulation, HAIL achieves approximately 2.5× more full-pipeline completion over an energy-efficient CGRA base-line.

# Motivation

- batteryless systems still fall short in two fundamental ways. 
	1) batteryless system do not provide timing dependability across widely varying harvesting contexts.
		-  To achieve timing dependability, 
			- a node must remain available for time-critical tasks even when:
				- harvested energy is scarce and highly variable, 
				- and must finish processing before data becomes stale [28], [29]. 
	2) Second, they cannot effectively exploit latent energy—harvested energy available during favorable conditions 
		- it remains unused because the node is ==**provisioned** only for a fixed worst-case operating point.==

- As a result, existing designs either preserve forward progress 
		- at very low performance, 
		- or achieve higher performance only 
	- in a best-effort manner without higher dependability. 

## Time-Varying Harvested Energy Motivates Intermittency- Aware Heterogeneity

- a node can support at any instant is tightly coupled to its current energy context [31], [32], [33]. 
- A node may only sustain minimal sensing and bookkeeping during weak intervals
	-  briefly support more aggressive processing when energy becomes available.
- single deployment can expose a node to both harsh and favorable energy regimes over time.
- More importantly, the latency of a critical sensing, inference, or communication pipeline depends not only on core execution time,
		- but also on recharge delay,
		- storage dynamics,
		- regulator efficiency,
	- and whether sufficient energy is available when the task is released [35], [28].
	- 
- DVFS alone provides only a narrow operating envelope 
	- roughly 1 mW to 200 mW [37]
	- 500 µW to7 mW in other batteryless DVFS designs [36].

- single processor is difficult to optimize simultaneously for low-power operation and high-performance execution using DVFS alone [38], [39].

- Low-power sub-threshold and near-threshold cores operate in the nW—µW range and preserve survivability under weak harvesting,
	-  limited throughput prevents them from completing compute-intensive pipeline stages within tight deadlines. 
- Conversely, CGRAs, accelerators, and super-threshold cores provide orders-of-magnitude can have:
	- higher throughput 
	- more energy efficient for demanding workloads,
	- downside: mW-level instantaneous power 
		- prevent them from booting or sustaining execution under weak harvesting.
## Dependability Challenges in Current Batteryless Design Approaches

- Designs optimized for a Worst-case single harvesting regime can't take advantage of latent energy 
- Designs optimized for best case senario is also terrible:

- batteryless latency is governed by more than compute speed
	- Recharge delay, PMU thresholds, cold-start overhead, storage capacity, leakage, regulator efficiency,and on-chip power-delivery losses all affect when a compute domain can turn on, how long it can remain on, and whether a timing-critical task can complete before its deadline 
	
- Thus, neither worst-case-only nor best-case-only provisioning provides dependable execution across dynamic harvested-energy contexts.

- low-power core is often the best choice for lightweight tasks 
	- because it can turn on quickly, preserve survivability, and meet the deadline.
	- while  higher-performance core may not have enough usable energy to boot

- the best compute substrate depends on both task demand and the energy state at the task arrival time.
- Examples of opportunistic work include 
	- additional feature extraction, higher-fidelity inference [50], local model refinement [51], encryption, data aggregation, and federated learning updates [52].


# HAIL Overview

HAIL addresses these requirements with a heterogeneousbatteryless architecture built from iso-ISA cores and compatible accelerators that span different power-performance regimes. 

- The iso-ISA organization preserves a common software interface across cores,
	- reducing code duplication and simplifying task migration, 
	- reducing software-management overhead in memory-constrained 

- while heterogeneity provides the range of power, latency, and energy-efficiency characteristics needed under intermittent power.
- Static core selection chooses a set of cores and operating points for a target deployment by:
	- jointly considering critical-task deadlines, 
	- harvested-energy availability, 
	- execution cost, 
	- power-delivery losses,
	- and area constraints.
- The runtime scheduler then maps each released task to the most appropriate currently feasible core or operating point though 
		- current energy state, task priority, remaining deadline, domain state, and predicted execution cost. 
	- It prioritizes deadline-constrained critical work and admits best-effort opportunistic work only when residual powered capacity remains.

- Together, these design-time and runtime mechanisms allow HAIL to improve dependable full-pipeline execution while converting otherwise latent energy into useful computation

## HAIL System Architecture 

- The architecture is organized into three voltage domains 
	- a low Vdd ultra-low-voltage domain for near-threshold, low-power operation,
	- a high Vdd compute domain for higher-performance cores and tightly-coupled accelerators [43],
	- a separate high Vdd I/O domain. 

- On-chip level shifters support safe communication between the low Vdd and high Vdd domains.  

- HAIL uses a dual-domain power architecture comprising 
	- an always-available low-power (LP) domain 
	- separate high-performance (HP) domain (reflects a battery based proposal [53], but system model extended for HAIL heterogeneity)


- managed by the LP domain that can be used by for the IO an high-performance domain. Each domain is backed by its own storage capacitor (a modified version of battery-based EH proposal [54] to support dual-domain power-neutral, energy-neutral operation) and governed by independent restart and die thresholds. 

- The LP domain is intended to remain reactive under weak harvested power, whereas the HP domain is activated only when sufficient energy

- The low-Vdd domain acts as the control plane for the node. It hosts ultra-low-power cores that execute:
		- lightweight critical tasks, interface with sensors, run the runtime scheduler, and control when the compute and I/O domains are enabled.
	- This role is especially important because turning on a batteryless domain from a fully de-energized state incurs a cold-start cost.


- A cold start occurs when the storage capacitor has recharged from below the die threshold to the restart threshold and the domain must re-enter operation from an unpowered state.
	- Before useful work can execute, the node must wait for the regulator and PMU to reach a stable operating region, initialize memories and peripherals, restore or reconstruct required state, and pay leakage, quiescent, and boot-energy overheads [55]. These costs consume both energy and time.
	- Because of this overhead, requiring the high-performance or I/O domains to cold start for every scheduling decision would waste scarce harvested energy, increase response latency, and risk missing short task deadlines before execution begins. Instead, HAIL keeps scheduling and power-management decisions in the low-Vdd domain, which can become available quickly under weak harvesting. 

- During system initialization, the LP-domain hardware enables the high-Vdd I/O domain only long enough to initialize the FRAM interface and stage runtime code into a small local instruction buffer. 
- The I/O domain can then return to idle, while the LP domain continues managing task dispatch and power-domain control. This avoids repeated high-energy regulator startups while keeping the node responsive under intermittent energy.

- The high Vdd compute domain provides higher-performance execution for demanding critical tasks and opportunistic work-loads.
	- It may include scalar cores, vector engines, vector register files, or accelerator-like fabrics. 

- Each compute element uses small scratchpad memory for active task state, while FRAM serves as the main non-volatile program and check-point storage. 

- Communication between LP and HP domains is organized through local memories, lightweight mailbox signaling, and DMA-assisted transfers (e.g., as provided by [53]), reducing cross-domain traffic and avoiding reliance on a single shared RAM.

- Harvested energy is stored in a dual-capacitor architecture.
	- A small capacitor powers thelow Vdd domain, enabling short restart times and dependable low-power operation. 
	- A larger capacitor powers the high Vdd compute domain and allows high-performance execution only when sufficient energy is available.

- The power delivery network can follows prior works [56], [18], [49]. Off-chip stages include a first-stage BQ25504 boost converter raises the harvester voltage while the compute domain uses an SVR for higher-load efficiency [57].

- The LP domain uses an on-chip LDO for low-current operation.

## Static Core Selection

- The static core selector determines, pre-silicon, which cores and operating points should be physically provisioned in a given HAIL SoC, as shown in Figure 3. 
	- Its inputs include a candidate:
	- core library
	- harvested-energy estimations for the target deployment
	- task deadlines 
	- area constraints
	- and power-delivery characteristics.


- For each critical task, the selector (as shown in Figure 5a) partitions a harvested-power trace representative of the estimates of the energy-harvesting, into deadline-sized windows w
	- and evaluates whether each candidate core can complete the task within both the available energy and the task deadline.

The selector estimates the energy available (power-delivery efficiency and leakage losses) and the energy needed to finish the task in that window. This produces a feasible fraction Fi,j, which captures how often core ci satisfies task Tj’s timing and energy requirements across the observed trace. A subset S ⊆C is valid only if every critical task reaches the target dependability threshold δ. Among valid subsets, HAIL selects a compact heterogeneous substrate that satisfies the area budget while maximizing coverage of feasible execution windows.

- HAIL searches for a set of complementary cores and operating points. 
	- Low-power cores preserve dependable progress under poor harvesting conditions,  
	- higher-performance cores convert favorable energy windows into timely completion of more demanding tasks.

The goal is to meet a target feasibility fraction for every critical task while maximizing the opportunity to use residual energy for best-effort computation.
## Runtime Task Scheduler

[53]An energy-efficient heterogeneous dual-core processor for internet of things,” in 2015 IEEE international symposium on circuits and systems

but extends the approach to intermittent batteryless operation by incorporating recharge

- delay, domain startup, capacitor state, energy loss due to PDN and leakage (static on-chip and capacitor). 

The scheduler treats each released pipeline stage as a deadline-constrained task and first checks whether it is pending, unexpired, and feasible on any available core or operating point.

- If no critical task can be safely scheduled and residual powered capacity remains, the scheduler admits opportunistic best-effort work.

- Critical tasks are considered before best-effort work. 
- Best-effort tasks are admitted only when doing so does not endanger the timely execution of critical tasks.
- scheduler is event-driven rather than continuously polling.