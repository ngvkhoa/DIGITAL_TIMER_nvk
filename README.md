# TIMER: Pure Logic Hardware Timer ⏱️

**A Microcontroller-Free, CMOS-Based Digital Timer**

This repository contains the documentation, schematics, and design principles for **TIMER**, a fully functional digital countdown and count-up timer implemented entirely at the hardware level. The system bypasses software abstraction and microcontrollers entirely, relying strictly on CMOS logic gates, physical timing circuits, and cascading counter logic.

## 📑 Project Abstract
The objective of this project is to demonstrate the fundamentals of digital logic and circuit design. The system features dual-mode operation (adjustable high-frequency count-up for configuration, and a precise 1Hz countdown), hardware-level switch debouncing, and robust signal routing. It addresses real-world hardware challenges, including floating inputs and electromagnetic interference (EMI), through strict hardware engineering practices.

## 🛠️ System Architecture

The circuit is divided into three primary functional blocks:

1.  **Clock Generation Module (NE556 Dual Timer):**
    *   **Timer A (Real-Time Clock):** Configured as an astable multivibrator utilizing a 68kΩ resistor and a 4.7µF capacitor to generate a precise 1Hz square wave for the countdown sequence.
    *   **Timer B (Configuration Clock):** Utilizes a 100kΩ variable potentiometer in series with a 1kΩ fixed resistor to generate a high-frequency (~20Hz) adjustable pulse train, allowing for rapid time configuration.

2.  **Control Logic & Signal Routing (CD4011 Quad 2-Input NAND):**
    *   **Hardware Debouncing:** RC networks paired with NAND gates act as Schmitt trigger equivalents to debounce the tactile inputs, ensuring clean, single-pulse signal transitions.
    *   **State Machine (SR Latch):** The NAND gates are configured as a Set-Reset (SR) latch. During the 'SET' phase, the high-frequency pulses are routed to the 'Clock Up' inputs. Upon asserting the 'START' signal, the latch toggles, isolating the setup clock and routing the 1Hz signal to the 'Clock Down' inputs.

3.  **Counting & Decoding Stage (4x CD40110BE):**
    *   Utilizes the CD40110BE, a monolithic CMOS IC integrating a decade up/down counter and a 7-segment display decoder. 
    *   The chips are cascaded to handle the carry/borrow logic automatically, ensuring accurate arithmetic transitions across the seconds and minutes digits (e.g., borrowing from the tens digit when passing zero).

## 🧮 Bill of Materials (BOM)

| Component Type | Part Number / Value | Quantity | Primary Function |
| :--- | :--- | :---: | :--- |
| **Integrated Circuit** | CD40110BE | 4 | Decade Up/Down Counter & 7-Segment Decoder |
| **Integrated Circuit** | NE556 | 1 | Dual Bipolar Timer (1Hz & Adjustable Clocks) |
| **Integrated Circuit** | CD4011 | 1 | Quad 2-Input NAND Gate (Logic & Routing) |
| **Display** | 7-Segment (Common Cathode) | 4 | User Interface |
| **Input** | Tactile Push Button | 3 | SET, START, RESET triggers |
| **Resistor** | 10kΩ | Multiple | Pull-down networks & debouncing |
| **Resistor** | 68kΩ | 1 | 1Hz Clock Timing |
| **Resistor** | 1kΩ | 1 | Baseline resistance for Setup Clock |
| **Potentiometer** | 100kΩ (Variable) | 1 | Setup Clock frequency adjustment |
| **Capacitor (Electrolytic)** | 10µF | 2 | Timing / General smoothing |
| **Capacitor (Electrolytic)** | 4.7µF | 1 | 1Hz Clock Timing |
| **Capacitor (Ceramic)** | 10nF (104) | Multiple | IC Power pin decoupling |

## 🛡️ Signal Integrity & EMI Mitigation

Transitioning from Proteus simulations to a physical PCB required specific design protocols to ensure stability due to the high input impedance of CMOS ICs:
*   **Pull-Down Networks:** All floating clock pins are tied to ground via 10kΩ resistors to prevent erratic "ghost counting" induced by ambient EMI.
*   **Pin Termination:** Unused input pins on the ICs are systematically tied to ground to ensure deterministic logic states.
*   **Decoupling:** 10nF ceramic capacitors are placed physically adjacent to the VCC and GND pins of every IC to suppress voltage transients and power supply noise.

## 👨‍💻 Author

**Nguyen Van Khoa**

*First-Year Engineering Student, Ho Chi Minh City University of Technology (HCMUT)*

*For inquiries regarding the schematic, simulation files, or PCB layout, please open an issue in this repository.*
