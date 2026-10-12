# BERTEL — Hardware-Based Digital Logic Word Game

**BERTEL** is an interactive, hardware-implemented word puzzle game inspired by Wordle. Built entirely using discrete transistor-transistor logic (TTL) integrated circuits, D flip-flops, and passive RC filtering networks, the platform evaluates 4-bit binary DIP-switch inputs (representing Hexadecimal characters) to guess a hidden 3-letter target word (`C-A-B`) without relying on microcontrollers or software code.

---

##  System Overview & Architecture

The system is split across four distinct, interconnected breadboard modules:
1. **Board 1 (Letter C - `1100`):** First character decoding and feedback logic.
2. **Board 2 (Letter A - `1010`):** Second character decoding and feedback logic.
3. **Board 3 (Letter B - `1011`):** Third character decoding and feedback logic.
4. **Board 4 (Control Module):** Handles Start, Reset, Life Counter (divide-by-two circuit), and Win state conditions.

### Feedback Mechanism
* **Green Light:** Correct character input in the correct slot position.
* **Yellow Light:** Correct character input, but belongs in a different slot position.
* **Red Light / Life Decrement:** Incorrect character input.

---

##  Logic Minimizations & Equations

Outputs were minimized using Boolean algebra theorems and Karnaugh maps (K-maps) to optimize gate counts across the prototype.

* **Board 1 (Letter C):**
  * Green Output: $L_G = W X Y' Z'$
  * Yellow Output: $L_Y = W X' Y$
* **Board 2 (Letter A):**
  * Green Output: $L_G = W X' Y Z'$
  * Yellow Output: $L_Y = W(X' Y Z + X Y' Z')$
* **Board 3 (Letter B):**
  * Green Output: $L_G = W X' Y Z$
  * Yellow Output: $L_Y = W Z' (X \oplus Y)$ *(Implemented using a 74LS86 XOR gate)*
* **Board 4 (Control System):**
  * Life Decrement Counter: $C_{LEDS} = (A' + B' + C') \cdot S$
  * Global Win Condition: $W_{LED} = A \cdot B \cdot C \cdot S$

---

##  Hardware Debouncing & Power Integration

* **Switch Debouncing:** Mechanical push-button switches feed into Resistor-Capacitor (RC) anti-bouncing sub-circuits to eliminate mechanical contact noise during clock pulse transitions.
* **Sequential Timing:** D Flip-Flop registers ($74LS74$) hold input states across clock pulses when the main `CHECK` button is pressed.

---

##  Bill of Materials (BOM) & Component Costs

Total prototype hardware cost: **$17.72**

| Component | Function | Quantity Used | IC Package Count | Unit Cost | Total Cost |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **74LS08** | Quad 2-Input AND Gate | 32 Gates | 10 ICs | $1.09 | $10.90 |
| **74LS32** | Quad 2-Input OR Gate | 3 Gates | 3 ICs | $0.79 | $2.37 |
| **74LS04** | Hex Inverter | 11 Gates | 4 ICs | $0.89 | $3.56 |
| **74LS86** | Quad 2-Input XOR Gate | 1 Gate | 1 IC | $0.89 | $0.89 |
| **Total** | | **47 Gates** | **18 ICs** | — | **$17.72** |

---

##  Repository Structure

```text
BERTEL-Digital-Logic-Game/
├── README.md
├── docs/
│   └── BERTEL_Design_Specification.pdf
├── schematics/
│   ├── board1_C_multisim.png
│   ├── board2_A_multisim.png
│   ├── board3_B_multisim.png
│   ├── board4_control_multisim.png
│   └── full_bertel_system_multisim.png
└── media/
    ├── integrated_breadboard_setup.jpg
    └── operation_demo.mp4
