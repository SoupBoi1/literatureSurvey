AsTAR, an energy-aware task scheduler that automatically adapts task execution rates to match available environmental energy.


 energy harvesting  is notoriously complex to develop reliable energy-harvesting applications due to three factors: 
1. Environmental dynamism, which gives rise to an unpredictable energy supply,
2. heterogeneity in platform and peripheral power demands 
3. [[the tragedy of the coulombs]] [10.1145/2809695.2809707]-  wherein a single energy-hungry software module can starve all other modules of energy, rendering the device inoperable.

Asymmetric Task Adaptation Rate scheduler (AsTAR)   throttling task execution rates to sustain good amount of charge. 
- Uses no pre-configuration or a priori modeling 
- Fine-grained control over task execution no ([[the tragedy of the coulombs]])
- AsTAR fairly schedules tasks based on user-defined priorities, their energy consumption or a weighted combination thereof.

AsTAR contributions
- (i) Simple, yet effective energy-aware task scheduling on IETF Class-1 devices 
- (ii) Support for platform heterogeneity and dynamism with zero up-front modeling, and 
- (iii) A reference platform for experimentation with sustainable energy-harvesting applications.

in AsTAR evaluation they show 
- (i) AsTAR achieves its goal of amassing an optimal charge level on heterogeneous hardware platforms.
- (ii) AsTAR adapts quickly to dynamic levels of power generation or consumption. 
- (iii) AsTAR fairly sched- ules tasks according to developer-specified priorities or the energy impact of those tasks. 
- (iv) AsTAR has extremely low performance overhead even on IETF Class-1 devices. 

# Related Work
Harvest 
- energy prediction model  is one of the bigger solutions being looked at 

- united federation of peripheral (UFOP) introduces multiple capacitors for each peripheral. 

- [[the tragedy of the coulombs]] : a single energy  hungry task consume sufficient energy to render the entire system inoperable. 
- engergy. hervestor reseach fall in two broad categories
	-  sustainable operation in the face of dynamic energy availability
	- Embraces intermittent operation
- sustainable is Astar 

OS
- ink mangeses memoery and timings in face of intermittent engergy while astar fouses on sustantablity 
- cinder OS added a virtuall lasy of engergy aviblity of each task/application 
- Eon uses Exponentially Weighted Moving Average (EWMA)  power supply perdictions while energy consumption estimates are based upon benchmarks obtained. 

Software

Econ is alanguage made for perpetually powered systems. Targets only server-class devices

reitter et al estimemte life cycle of IoT

Gaps. and requerments 

- Reducing the burden of benchmarking
- operation on heterogeneous: support ever changing hardware

- Flexible Multi-Tasking: 
	- task excualtion on  developer-specified priority vs energy consumption
	- Astar say why not both 
- Mitigating dynamism:  respect unpredictable environments, **long term modeling is likely proven in effective**  
- low runtime overhead
``



# Methodology



# Implementation


# Evaluation


# Citation
[[AsTAR Citations]]


![[3467894.pdf]]



