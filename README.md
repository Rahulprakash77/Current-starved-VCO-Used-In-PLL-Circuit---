#  Current starved VCO ( Used In PLL Circuit -  )
This project focuses on the design and implementation of a Current Starved Voltage Controlled Oscillator (VCO) for use in a Phase-Locked Loop (PLL) circuit. The VCO plays a crucial role in generating the output frequency, which is locked to a reference signal, making it essential for applications requiring stable and precise frequency synthesis.

### Why this project is important ?
This project is important because the Voltage Controlled Oscillator (VCO) is a fundamental building block in many electronic systems, particularly in Phase-Locked Loops (PLLs), where it serves as the frequency generator. A Current Starved VCO offers advantages like low power consumption, improved noise performance, and simplified circuit design compared to other types of VCOs. Understanding and optimizing the performance of the Current Starved VCO can lead to advancements in various fields such as wireless communications, radar systems, and frequency synthesizers. Additionally, this project provides hands-on experience in analog circuit design, simulation, and optimization, which are valuable skills for electronics engineers. Finally, by exploring the intricacies of PLL circuits, this project offers insights into frequency stability, phase noise, and signal synchronization, which are critical for many modern electronic devices.

### Final Problem statement
8x PLL Clock Multiplier PLL Design with an input frequency range of 5Mhz to 12.5Mhz and output frequency range of 40Mhz to 100Mhz, giving an 8x multiplied clock at ~50% duty cycle on tt corner at room temperature. 

##  PLL Theory and MY design specification 

PLL is a control system implemented using an analog circuit, which is used in many applications like, FM modulation and demodulation circuits, motor speed controls and tracking filters, frequency shifting decodes for demodulation carrier frequencies, time to digital converters, Jitter reduction, skew suppression, clock recovery, to get a precise clock signal without frequency or phase noise. PLL actually mimics the reference means to have the same or a multiple of the reference frequency and a constant phase difference with it. In this workshop, our focus was to design a PLL for clock signal operations in different SoCs and ICs. PLL uses a Voltage Controlled Oscillator and gives an output that has superior spectral purity nearly equal to that of Quartz Crystal.

## PLL components

PLL has five main components, and they are:-

  ### (1) Phase Frequency Detector
  ### (2) Charge Pump
  ### (3) Low Pass Filter
  ### (4) Voltage Controlled Oscillator
  ### (5) Frequency Divider


![pll_components](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/76118957-78dd-42c9-81b3-74fb76ddb101)


## (1) Phase Frequency Detector

It compares the phase difference between the input signal and output signal and generates a pulse signal according to it. The width of the pulse is used to determine the phase of the signal. As for width increases, phase also increases. The PFD output voltage is used to control the VCO such that the phase difference between the two inputs is held constant, making it a negative feedback system. The dead zone is one of a problem with PFD. The phase difference below which PFD output is not able to reach the desired logical level and fails to turn on the charge pump switches is called the dead zone. To tackle this, one should use precision PFDs, which overcome this issue.

![pfd](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/9206e1b8-b42b-433b-8c72-a2b81780fdd5) ![pfdfunc](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/7169d6fc-fe51-4b89-a0cc-3ee22fe6040c)




## (2) Charge Pump

The PFD generates a digital signal, but VCO uses an analog signal as input. So, here comes Charge Pump in the picture. The charge pump converts the digital signal of PFD into analog control signal which is given as input to the VCO. This can be done using the current steering circuit. When the UP signal is active the capacitor gets charged, this increases the voltage at charge pump output. When the DOWN signal is active, the capacitor gets discharged through the ground. This output voltage controls the VCO. An increase in voltage, speeds up the oscillator, while a reduction in voltage, slows down the oscillator.

![cp](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/3e13882c-fd6c-4b04-bb83-b10892cbbc48) ![cp_func_up](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/5b4552a5-c89b-43cd-bfe7-004d08423329) ![cp_func_down](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/77749c3c-6d2e-4fca-a6b0-16f5a2a756c1)




## (3) Low Pass Filter

The main issue with the charge pump is charge leakage, it keeps charging the capacitor even when inputs are off. This is tackled by using Low Pass Filter in the output. After that, the low-frequency dc voltage signal is sent into a dc amplifier, which boosts the signal level. After that, the VCO receives the enhanced signal. This will smoothens the output, without LPF, PLL cannot lock and mimic the ref signal. Even though, the primary function of the LPF is to maintain the stability of the system. Mathematically, only one capacitor at the output of the Charge pump makes the system unstable because if we see the frequency domain analysis of the circuit then there will be two poles in the transfer function which makes it oscillating and highly unstable. Thus an RC LPF is added at the output such that output gets stabilize. The value of Cx should be roughly around one-tenth of C. The loop filter bandwidth must be less than one-tenth of the highest output frequency.
![lpf](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/e2057faf-2cfa-4651-838a-3cd027ada62d)


## (4) Voltage Controlled Oscillator ( current starved VCO )

VCO is the heart of PLL and it generates the desired high-frequency clock output. Voltage-controlled oscillators are the actual parts that produce alternating digital clock signals. The most common one is the Ring oscillator. It contains an odd number of inverters and flips the output. So, VCO can be implemented using simple inverters. It is necessary to design this VCO such that the range of output frequency we want for the PLL is within the range of frequency the VCO can produce properly. The frequency of this clock signal can be controlled by the input voltage. The frequency depends on delay and delay depends on the current supplied. The LPF output serves as a VCO control signal. Current sources are used at the top and the bottom with the Vctrl voltage to control the ring oscillator. An analog signal is produced by the VCO and its amplitude is proportional to the LPF output amplitude.
![vco](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/7f449d5d-1013-4fba-888f-c886dec5378e)


## (5) Frequency Divider

A frequency divider is used to convert the whole system into a frequency multiplier. A PLL with a frequency divider on its feedback loop is called a clock multiplier PLL. Such a PLL can make clock signals which are multiples of the reference signals. A toggle flip flop generates the clock which has twice the time period of the input clock given to it, that is we obtain the clock having half the frequency of the input clock provided to the frequency divider. For the 8x clock multiplier, we should divide the output clock by 8 to generate a feedback clock. For obtaining one-eighth of frequency, we should cascade three toggle flip flops. When the frequency difference of each input is 0, which shows a consistent phase difference, the loop is locked. If the two signals are shifted by 180, the output voltage must be at the highest. The output voltage generated will be zero in the absence of an input signal to allow the VCO to function at a fixed frequency. This frequency is referred to as the oscillator's operating frequency. Lock Range: The range of frequencies for which PLL can maintain lock, already in the locked state. Capture Range: The frequencies for which PLL is able to lock in from an unlocked state. Settling Time: The time within which the PLL is able to lock in the form of an unlocked condition.

![fd](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/5e48cd27-431e-4a53-9ce2-87d1bd79f853)



#### Specifications

    Technology Node = 90nm
    Supply voltage (VDD) = 1.8V
    Minimum Input Frequency = 5MHz
    Maximum Input Frequency = 12.5MHz
    Multiplier = 8 times
    Jitter (RMS) < 20ns
    Duty cycle = 50%
    tool: cadence 
![Untitled](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/6c7220ea-5853-4f40-afd9-bd7d88e5ac70)


#### Design Flow

    Specifications of the IC
    SPICE level circuit development
    testing full circuit
    
   

##  PLL Design full circuitory

 we are going to design different components of PLL at the transistor level, then we will simulate it in Ngspice. Check all the desired outputs, and then proceed with Layout in Magic followed by parasitics extraction. After all this, we will again do the post-layout simulation in Ngspice. After all of this, we will combine different components of PLL in a single layout design. At last, we will integrate the layout design of PLL with Caravel SoC. In the end, we will discuss about tape-outs, and know-how any individual can send their own layout design to The Google-Skywater open MPW shuttle program, to be manufactured on a silicon wafer. But it depends on the choice of Google and Skywater to whether manufacture or not.
PLL components circuit design

The transister level design of Phase Frequency Detector, Charge Pump, and Voltage Controlled Oscillator, respectively are as follows:

### PLL components circuit simulations

![pfd](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/573000ac-b7df-4bc9-b61d-5fb78fe4ad92)

![cp](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/dc608e85-0bfb-4e6d-91e1-dec3b7386f02)

![vco](https://github.com/Rahulprakash77/PHASE-LOCK-LOOP-PLL-DESIGN-/assets/130161648/5c01c40f-5af7-46af-b648-f7a4e28095e0)





### Extra Reference Material
    i have followed these two reference paper  ( https://www.eecis.udel.edu/~vsaxena/courses/ece504/Handouts/Razavi1996_PLL_IEEExplore.pdf)
     (https://ieeexplore.ieee.org/document/7755299)
    Analog IC design By Razavi 
    analog ic design by gray and Hust
    NPTEL Course on Analog IC Design by Nagendra Krishnapura
    NPTEL Course on RF Integrated Circuits by Shouri Chatterjee
    An Improved Performance Ring VCO: Analysis and Design
    A Novel Phase Frequency Detector for a High-Frequency PLL Design
    A pA-leakage CMOS charge pump for low-supply PLLs
    A simple and high performance charge pump based on the self-cascode transistor
    Gain-Boosting Charge Pump for Current Matching in Phase-Locked Loop
    
