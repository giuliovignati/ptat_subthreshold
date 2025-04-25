# ⚡ Voltage Reference with subthreshold MOSFETs
![Image](https://github.com/user-attachments/assets/1dc5d9cd-48ef-45cc-8156-f44592f1d1de)
---

## 🎯 Project Objectives 
The objective is to design a voltage reference of 500mV using MOSFETs operating in the subthreshold region, with a channel length ranging from 50nm to 300nm. The design should minimize variations in the output voltage $V_{ref}$ over a temperature range of -20°C to +80°C.

## 🔥 PTAT in Subthreshold Region

In the **subthreshold (or weak inversion)** region of a MOSFET, the **drain current increases exponentially with the gate-source voltage** and has a strong **temperature dependence**. This can be exploited to create **PTAT (Proportional To Absolute Temperature)** behavior, useful in temperature sensors and analog biasing.
The subthreshold drain current is approximately:

$$
I_D = I_0 \cdot \exp\left(\frac{V_{GS} - V_{TH}}{n \cdot V_T}\right)
$$

Where:

- $I_0$ is a pre-exponential factor (process-dependent).
- $V_T = \frac{kT}{q}$ is the thermal voltage, which is proportional to **absolute temperature** ($T$).
- $n$ is the subthreshold slope factor (typically 1.1–1.5).
- $V_{GS}$ and $V_{TH}$ are the gate-source and threshold voltages, respectively.

---


## 🧭 Project Overview

#### 📏 Project Costraints

| $V_{dd}$ | $V_{ref}$ | $I_{dd}$ | $R_{max}$ | $DVR1$          |
|----------|-----------|----------|-----------|-----------------|
| $1V$     | $0.5V$    | $100nA$  | $≤20kΩ$   | $50-100mV$      |

For semplicity we'll assume 
| Thermal Voltage $V_{TH}$ | Subthreshold Slope $n$ |
|----------|-----------|
| $25mV$     | $1.2$    |

## ➗ Case of study Equations

- $V_{\text{ref}} = V_s \left( \frac{R_3 + R_4}{R_4} \right)$
- $I_{D3} = \frac{V_{GS4} - V_{GS3}}{R_1} = \frac{n \cdot V_{TH} \cdot \ln(N)}{R_1}$ 
- $V_s = V_{GS4} + 2 \cdot I_{D3} \cdot R_2$ 
- $\Delta V_{R1} = R_1 \cdot I_{d3}$

## 💻 Spice Simulations:
#### Schematic
![Image](https://github.com/user-attachments/assets/b4bb2f84-02bb-4a84-bbcf-e283fb66d3f0)

## M1 - M2 Mirror Design
M1 and M2 are designed with a $W=1u$, $L=150n$, it is important no notice that are implemented with extended channel lengths to maximize output resistance, minimize channel-length modulation and mismatch, and thus serve as a highly stable, precision current source.
#### M1, M2 Drain Currents Ids
The slightly difference between the IDS is justified by the fact that M1 is diode-connected ($VDS1=VGS1$) while M2 runs at whatever VDS the core node demands.
![Image](https://github.com/user-attachments/assets/0d1750ca-0d4b-4c46-92a7-2b07796115c2)
#### M1, M2 Gate Currents Ig
It was rather interesting plotting the gate-tunneling currents, given the fact that we're using a nanometric technology.  
![Image](https://github.com/user-attachments/assets/48fd5c68-f65c-4f99-8f1e-d8080e473d50)

## M3 - M4 - R1 - R2 Design 
M3 and M4 are designed with a $W=1u$, $L=55n$, it is necessary to have an $m > 1$ given that we want M3 more conductive than M4 (the presence of $R1$ makes $Vgs3 < Vgs4$).

The following $DVR1$ and $I3$ were chosen:

- $\text{DVR1} = 78 \text{mV}$  
- $\text{I3} = 30 \text{nA}$

From those we can derive $R1$:  
- $R1 = \frac{\text{DVR1}}{\text{I3}} = 2.6 \text{M}$

At this point we are able to obtain $N$ (which is equal to $m$ molteplicity factor):
- $N = e^{\frac{I_{D3} \cdot R_1}{n \cdot V_{TH}}} = 11$

Now we are able to sweep R2 (with a .step function) in order to find a suitable value that guarantees: 
- $\frac{dV_{ref}}{dT} \approx 0$

The best value found for this is $R2 = 1.63Meg$


## M5 - R3 - R4 Design 
M5 was designed with a $W=1u$, $L=55n$ similarly to M3 and M4.
R3 and R4 were chosen with the objective of amplifying $V_s$ in order to get $V_{ref} = 500mV$ according to $V_{\text{ref}} = V_s \left( \frac{R_3 + R_4}{R_4} \right)$.
$R3 = 8.25Meg$ and $R4 = 20Meg$ satisfy this costraint, and have a sufficiently large value that guarantee that M5 stays in subthreshold region.
#### Drain Current of M5 to certify that it is in the subthreshold region
![Image](https://github.com/user-attachments/assets/f2d3845a-6878-46c7-841a-4102a54feaf3)

--- 


## 📊 Results

#### $Vref$ analyzed on a full temperature sweep -20°C, +80°C
![Image](https://github.com/user-attachments/assets/5a07c5ac-c4e5-47ca-964e-f4c57221e9cd)
#### Total Current Idd
![Image](https://github.com/user-attachments/assets/3d6a83c1-8076-40c5-81fd-d8e48ccb60f0)

---

## 🔄 Further Optimization and overall final results

The obtained Idd at 80°C is $98.2nA$, so it is very close to $100nA$.
We could think of using a lower $N$, in particular $N = 8$ in order to obtain a lower $Id3$.

#### Revisited Schematic with updated values
![Image](https://github.com/user-attachments/assets/1a67a2fc-60bf-465a-96ed-778d25a329a6)

The new values for R2, R3 and R4 are listed below (these are obtained with the exact same calculus as before):

- $R2 = 1.85Meg$ 

- $R3 = 8Meg$ 

- $R4 = 19.5Meg$

#### $Vref$ analyzed on a full temperature sweep -20°C, +80°C
![Image](https://github.com/user-attachments/assets/e7e5ed28-71b7-4998-ab03-4d566da90c57)

#### Total Current Idd
![image](https://github.com/user-attachments/assets/1bf92b92-b68c-4e91-bf5d-3d82a5bd6c49)

