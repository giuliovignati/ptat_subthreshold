# Voltage Reference with subthreshold MOSFETs
![Image](https://github.com/user-attachments/assets/1dc5d9cd-48ef-45cc-8156-f44592f1d1de)
---

## Project Objectives 
The objective is to create a voltage reference of 500mV using mosfet with channel length in the range 50nm - 300nm in subthreshold region with a minimum variations of the output voltage in the temperature range -20, +80 celsius degrees.

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


## Project Outline
$Vdd = 1V$
$V_{ref} = 0.5V$
$Idd = 100nA$
$R \leq 20\mathrm{k}\Omega$
DVR1 = [50-100mV]



*Fine del documento.*
