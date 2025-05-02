# 📚 Learning BGR (Bandgap Reference) Design — Progress Log

This repository documents my learning journey into designing **Bandgap References (BGR)**, based on lectures by **Hafeez KT** and practical implementation using **LTspice**.

---

## 🔧 Block Diagram Overview

A **Bandgap Reference (BGR)** is a temperature-independent voltage source, essential for robust analog and mixed-signal circuits.
![image](https://github.com/user-attachments/assets/24a2fd01-15dc-44a0-9c22-aa5387248a35)

### 🧠 Conceptual Block Diagram

```
 V_PTAT → α1 Gain → +
                       \
                        → Summer → V_BGR
                       /
V_CTAT → α2 Gain → -
```

---
![image](https://github.com/user-attachments/assets/51b79020-4c58-4f5e-92c9-dd09882ff50d)

## 📝 What is a Bandgap Reference?

A **Bandgap Reference (BGR)** provides a stable voltage (\~1.2 V) that is independent of **temperature**, **supply voltage**, and **process variations**.

### 📌 Applications of BGR:
It is used almost in every IC
* Analog-to-Digital Converters (**ADCs**)
* Digital-to-Analog Converters (**DACs**)
* Voltage Regulators
* Oscillators
* Temperature Sensors

---

## 🌡️ Industry Standard Variations

| Parameter             | Typical Range     |
| --------------------- | ----------------- |
| **Temperature**       | –40 °C to +125 °C |
| **Supply Variations** | ±10% to ±20%  (It can vary according to applications) |

These are crucial for **automotive and industrial** reliability.

---

## 📈 Temperature Dependencies

![image](https://github.com/user-attachments/assets/2cf7f024-0078-4ac1-9c3f-aa5a61776151)


### CTAT – *Complementary to Absolute Temperature*

* **Decreases with temperature**

### PTAT – *Proportional to Absolute Temperature*

* **Increases with temperature**

### ZTAT – *Zero Temperature Coefficient*

* **Flat/constant with temperature**
* Achieved by combining CTAT and PTAT



---

## ⚙️ How We Generate a Constant Voltage (BGR)

We combine CTAT and PTAT voltages such that their **temperature effects cancel**, producing a stable **ZTAT output**.

$$
V_{\text{BGR}} = \alpha_1 \cdot V_{\text{PTAT}} + \alpha_2 \cdot V_{\text{CTAT}}
$$

* $\alpha_1$, $\alpha_2$: **scaling factors** adjusted to cancel temperature coefficients.

### 📊 Graphs

* CTAT: ↓ with temperature
* PTAT: ↑ with temperature
* BGR/ZTAT: ≈ constant

---

## 🧪 CTAT Generation — Circuit Insight
![image](https://github.com/user-attachments/assets/94ec54e9-04e8-4a48-a8cd-55a4e6dd933f)

### 🔋 Using a Constant Current Source & Diode

A **constant current source** biases a **diode-connected BJT**, generating a voltage that **decreases with temperature**.

---

### 💡 Diode Implementation in CMOS

CMOS doesn’t offer standalone diodes. Instead, we exploit **parasitic BJTs**.

---

### 1️⃣ NPN-Based Diode (Common)

* Formed from:

  * **Collector**: n-well
  * **Base**: p-substrate (GND)
  * **Emitter**: n+ diffusion

#### ✅ Diode-Connected Configuration:

* **Emitter and Base**: Grounded
* **Collector (n-well)**: Biased by current → **CTAT voltage output**

```plaintext
      Vout (Collector)
         |
         |
       |/
GND ---|  NPN BJT (diode-connected)
       |\
        |
       GND (Emitter, Base)
```

---

### 2️⃣ PNP-Based Diode (Also Possible)

* Formed from:

  * **Emitter**: p+ diffusion in n-well
  * **Base**: n-well
  * **Collector**: p-substrate (ground)

#### ✅ Diode-Connected Configuration:

* **Base and Collector**: Connected to **GND** (p-substrate)
* **Emitter**: Connected to bias current → Output is **V\_EB (CTAT)**

```plaintext
           Vout
            |
        Emitter (p+ in n-well)
            |
           |\
           |  PNP (diode-connected)
           |/
            |
      Base = Collector → GND (p-substrate)
```

> 🔍 **Note**: While PNPs are harder to use due to low gain and lateral layout, they are sometimes employed for **CTAT voltage generation**, especially in processes lacking good NPNs.

---

## 📐 Temperature Coefficient Equation
![image](https://github.com/user-attachments/assets/e82c5509-0562-48e6-9bf2-47b026e21832)


The diode's temperature behavior is governed by:

$$
\frac{dV_D}{dT} = \frac{V_D - V_T(4+m) - \frac{E_g}{q}}{T}
$$

From textbooks and experiments:

$$
\frac{dV_D}{dT} \approx -1.66\,\text{mV/K}
$$

This confirms CTAT behavior.

---

## 🧰 LTspice Simulation

### Setup:

* Current source feeding diode-connected BJT (NPN or PNP)
* Temperature sweep:

  ```
  .step temp -40 125 1
  ```
  ![image](https://github.com/user-attachments/assets/d2c5cc05-72a7-4deb-867a-f67698a9318b)


### Output:

* **CTAT Curve** showing a negative slope:
  ![image](https://github.com/user-attachments/assets/868c825b-2ef5-4f3a-9563-94ef586bc0ba)

* **Slope Analysis (\~–1.6 mV/°C)**:
  ![image](https://github.com/user-attachments/assets/777c2d4a-674e-41ed-910f-7bd07dc7d034)

---

## 🧠 Key Observations

* CTAT voltage decreases linearly with temperature.
* Whether using NPN or PNP, the diode voltage tracks temperature with known slope.
* Final BGR design relies on **scaling PTAT and CTAT** to achieve flat output.

---

## 📎 References

* Hafeez KT – [YouTube Series on BGR Design](https://youtube.com/playlist?list=...)
* Grey & Meyer – *Analog Integrated Circuit Design*
* Razavi – *Design of Analog CMOS Integrated Circuits*

---
