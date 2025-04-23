# Voltage Reference con MOS Sottosoglia

**NB**: Il comportamento del MOSFET in sottosoglia è analogo a quello del BJT per andamento esponenziale.

---

## 1. Descrizione del circuito

![Circuito di riferimento](schematic.png)

- **M1** e **M2** hanno $W/L$ uguale.  
- **M3** è dimensionato in modo da avere un coefficiente di amplificazione maggiore di M1, in modo da stabilire le opportune tensioni $V_{GS2}$ e $V_{GS3}$ grazie alla presenza di $R_1$.

La tensione di riferimento si ricava da:
  
$V_{\text{REF}} = V_S \,\frac{R_3 + R_4}{R_4}$ \quad\longrightarrow\quad \text{(usiamo $R_3$ e $R_4$ per shiftare $V_S$ verso l'alto).}$

Vogliamo che $V_{\text{REF}}$ (e quindi anche $V_S$) sia **indipendente** dalla temperatura.

---

## 2. Corrente in sottosoglia (MOS)

In regime sub-threshold un MOSFET presenta la stessa dipendenza esponenziale di un BJT:

$I_{DS} = k_0\,I_T\,\exp\left(\frac{V_{GS}-V_{TH}}{n\,V_T}\right)$

dove:

- $I_T = \frac{k\,T}{q}$ è la corrente termica,  
- $V_T = \frac{k\,T}{q}$ è il potenziale termico,  
- $n$ è il coefficiente di subthreshold-slope,  
- $k_0 = N\,k_1$ tiene conto di $N$ dispositivi in parallelo.

Per esempio, per M4:

$I_{D4} = k_1\,I_T\,\exp\left(\frac{V_{GS4}-V_{TH4}}{n\,V_T}\right)$

e analogamente per M3:

$I_{D3} = k_1\,I_T\,\exp\left(\frac{V_{GS3}-V_{TH3}}{n\,V_T}\right)$

Poiché:

$V_S = V_{GS3} + I_{D3}\,R_2,$

il contributo di $V_{GS3}$ deve compensare la variazione termica di $I_{D3}$ per rendere $V_S$ **indipendente** dalla temperatura.

---

## 3. Tensione di soglia del MOS

La tensione di soglia su CMOS è:

$V_{TH} = \frac{\sqrt{2\,q\,N_a\,\varepsilon\,(2\phi_f)}}{C_{ox}} + 2\phi_f + \phi_{MS} - \frac{Q_{ss}}{C_{ox}}$

dove:

- $\phi_f = V_T \ln\left(\frac{N_a}{n_i}\right)$  
- $n_i^2 = N_c\,N_v\,e^{-E_g/(kT)}$  
- $\phi_{MS}$ potenziale metallo-substrato  
- $Q_{ss}$ cariche superficiali  
- $C_{ox}$ capacità di ossido per unità di area

Definendo:

$x = \sqrt{\frac{2\,q\,N_a\,\varepsilon}{C_{ox}}}$

si riscrive in forma compatta:

$V_{TH} = x\,\sqrt{2\phi_f} + 2\phi_f + \phi_{MS}$

---

### 3.1 Dipendenza termica di $V_{TH}$

$\frac{dV_{TH}}{dT} = \left[\frac{x}{2\sqrt{2\phi_f}} + 1\right]\,\frac{d(2\phi_f)}{dT}$

con:

$\frac{d(2\phi_f)}{dT} = \frac{2\phi_f}{T} - \frac{E_g}{4\,q}$

Il primo termine $\frac{2\phi_f}{T}$ è **positivo**, il secondo $\frac{E_g}{4q}$ è **negativo**: la loro somma determina il coefficiente di temperatura di $V_{TH}$.

---

## 4. Obiettivo di progetto

L’aumento di $V_{TH}$ con la temperatura (e quindi di $V_{GS}$) causa una variazione di $V_S$ e di conseguenza di $V_{\text{REF}}$.  
**Obiettivo**: progettare il set-up in modo che la derivata termica complessiva sia nulla nel punto di lavoro (tipicamente $20\,^\circ\text{C} \div 30\,^\circ\text{C}$).

---

## 5. Circuito di _startup_

**NB**: Senza un circuito di _startup_, esiste un secondo punto di riposo in cui tutti i transistor sono spenti (se M1 e M2 vedono $V_{GS}=0$, non ci sono cadute su $R_1,R_2$ e $V_S = 0$).  
Per forzare l’accensione all’avvio si aggiungono MS1, MS2, MS3:

![Circuito di startup](startup.png)

---

## 6. Dimensionamento esemplificativo

Parametri di progetto per una tecnologia CMOS (es. TSMC):

- $V_{DD} = 1.0\text{ V}$  
- $V_{\text{REF}} = 0.5\text{ V}$  
- Range di temperatura: $-20^\circ\text{C} \div +80^\circ\text{C}$  
- $I_{\text{REF}} = 100\text{ nA}$ a temperatura ambiente  
- $R_1 = 20\text{ M}\Omega$  
- Variazione di $V_{\text{REF}}$ in temperatura: $50\div100\text{ mV}$

---

*Fine del documento.*
