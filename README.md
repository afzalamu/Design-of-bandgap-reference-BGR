# 📚 Learning BGR (Bandgap Reference) Design — Progress Log

This repository documents my learning journey into designing **Bandgap References (BGR)**, based on lectures by **Hafeez KT** and practical implementation using **LTspice**.

---

## 🔧 Block Diagram Overview

A **Bandgap Reference (BGR)** is a temperature-independent voltage source, essential for robust analog and mixed-signal circuits.

### 🧠 Conceptual Block Diagram

```
 V_PTAT → α1 Gain → +
                       \
                        → Summer → V_BGR
                       /
V_CTAT → α2 Gain → -
```

---

## 📝 What is a Bandgap Reference?

A **Bandgap Reference (BGR)** provides a stable voltage (\~1.2 V) that is independent of **temperature**, **supply voltage**, and **process variations**.

### 📌 Applications of BGR:

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
| **Supply Variations** | ±10% to ±20%      |

These are crucial for **automotive and industrial** reliability.

---

## 📈 Temperature Dependencies

### CTAT – *Complementary to Absolute Temperature*

* **Decreases with temperature**
* Example: $V_{BE}$ of a diode or BJT

### PTAT – *Proportional to Absolute Temperature*

* **Increases with temperature**
* Typically generated using differences in $V_{BE}$

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
