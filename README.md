# DC-DC Buck Converter Design and Implementation

**Course:** EE 252: Electrical Machines and Power Electronics Lab (EMPEL)  
**Institution:** Indian Institute of Technology (IIT) Indore  
**Batch & Group:** B1-G5  
**Team Members:** Aaradhya Sharma, Gadgil Rucha, Bhasuru Nikhil, Maddi Harshini, Ankita Yadav  

---

## 📌 Project Overview
This project involves the hardware realization and analysis of a non-isolated DC-DC Buck Converter. The converter is designed to step down a higher input DC voltage to a lower output DC voltage by rapidly switching a power MOSFET. The control signals are generated using a TL494 PWM controller.

### ⚙️ Design Specifications
* **Input Voltage:** 10 V
* **Output Voltage:** 2 V to 8 V (Variable)
* **Switching Frequency:** 20 kHz (Continuous Conduction Mode - CCM)
* **Output Current:** 1 A
* **Operating Modes:** Continuous Conduction Mode (CCM) and Discontinuous Conduction Mode (DCM) (demonstrated by reducing the switching frequency).

---

## 🧰 Hardware Components

The system is structurally divided into two main stages: the Driver Circuit and the Power Circuit.

### 1. Control & Driver Circuit
The driver circuit uses the **TL494 IC** to generate the Pulse Width Modulation (PWM) signals, which are then fed into the **TC4428A MOSFET driver IC**. The driver amplifies the signal to accurately drive the gate of the MOSFET, ensuring rapid turn-on and turn-off characteristics. The duty cycle and switching frequency are fully adjustable via onboard potentiometers.

### 2. Power Circuit Stage
The power stage executes the actual voltage step-down process using high-frequency switching.
* **Power Switch:** IRFZ44N MOSFET 
* **Freewheeling Diode:** QH08TZ600 (Fast Recovery Diode)
* **Energy Storage (Inductor):** 1 mH Toroidal Inductor (Max rating 2.4 A)
* **Output Filter:** 470 µF, 63 V Electrolytic Capacitor & 100 µF, 25 V Smoothing Capacitor
* **Load:** 40 Ω, 3 A Variable Rheostat

---

## 📊 Performance & Efficiency Analysis

While the theoretical efficiency of an ideal buck converter ($V_{out} \cdot I_{out} = V_{in} \cdot I_{in}$) is 100%, the practical efficiency achieved in this hardware prototype is approximately **67%**.

This deviation from ideal efficiency is primarily driven by the **non-idealities of the Power MOSFET and the Freewheeling Diode**:

### 1. Diode Non-Idealities & Thermal Management (Heat Sink)
* **Conduction Losses:** The fast recovery diode provides a path for the inductor current when the MOSFET is turned off. Because the output voltage can be set as low as 2V from a 10V input, the duty cycle ($D$) can be quite low (down to 20%). This means the diode is conducting for a large portion ($1-D$) of the switching cycle. The power dissipated by the diode is proportional to its forward voltage drop ($V_f$) multiplied by the continuous load current. 
* **Reverse Recovery Losses:** When the MOSFET turns back on, the diode takes a brief moment to stop conducting, drawing a reverse recovery current that causes additional power dissipation.
* **Heat Sink Implementation:** Due to these heavy conduction and switching losses, the diode dissipates significant thermal energy. **A heat sink was specifically attached to the diode** to manage this heat, prevent thermal runaway, and ensure the stable operation of the component during prolonged testing.

### 2. MOSFET Non-Idealities
* **Conduction Losses ($I^2R$ losses):** Even when fully turned on, the IRFZ44N MOSFET has a finite drain-to-source on-resistance ($R_{DS(on)}$), causing power to be dissipated as heat while current flows from the source to the load.
* **Switching Losses:** The MOSFET does not turn on and off instantaneously. During the brief transition periods in every 20 kHz switching cycle, both the voltage across the switch and the current through it are non-zero simultaneously. This overlap creates significant power loss.

*Note: Additional minor losses stem from the Direct Current Resistance (DCR) of the 1 mH inductor's copper windings and the Equivalent Series Resistance (ESR) of the output capacitors.*

---

## 📈 Waveforms & Results
The operational integrity of the converter was verified using an oscilloscope. The following key waveforms were captured and analyzed:
1. **CCM Operation:** Inductor current and switch voltage at 20 kHz.
2. **DCM Operation:** Demonstrated by intentionally reducing the switching frequency until the inductor current reached zero before the end of the switching period.
3. Output current and smoothed output voltage ripples under load.

---

## 🚀 How to Use / Test
1. **Isolation:** Ensure the power supply for the gate drive (set at +12 V) and the power circuit ground are electrically isolated.
2. **Startup:** Set the load rheostat appropriately before switching on the power. Do not power the circuit without a load connected.
3. **Observation:** Use an oscilloscope (with differential probes if observing multiple ungrounded voltages simultaneously) to measure the switch current, diode voltage, and inductor ripples.
