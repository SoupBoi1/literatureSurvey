AsTAR, an energy-aware task scheduler that automatically adapts task execution rates to match available environmental energy.


 energy harvesting  is notoriously complex to develop reliable energy-harvesting applications due to three factors: 
1. Environmental dynamism, which gives rise to an unpredictable energy supply,
2. heterogeneity in platform and peripheral power demands, and 
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


# Methodology


# Implementation


# Evaluation


# Citation
[[AsTAR Citations]]


![[3467894.pdf]]



