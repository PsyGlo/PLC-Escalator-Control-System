# 🚀 PLC Escalator Control System (Demand-Driven)

This repository features a robust escalator control solution developed in **Schneider Control Expert**. It utilizes a Master/Slave Grafcet architecture and dedicated Ladder safety logic to balance energy efficiency with passenger safety.

---

### 🧠 Logic Strategy
The system is divided into three functional layers:

1. **Section 1: G_MEMO (SFC)**
   * Functions as a retriggerable memory.
   * Uses step **S_1_2** and transition `t#15s / S_1_2.X` to track passengers.
2. **Section 2: G_MOTEUR (SFC)**
   * Slave chart controlling the motor (**KM1**).
   * Synchronized via the activity bit `S_1_2.X`.
3. **Section 3: Init_Logic (Ladder)**
   * Implements the `INITCHART` function.
   * Ensures a clean system reset on Power-Up (`%S1`) or when the **BPA** (Stop Button) is pressed.

---

### ⚙️ I/O & Variables
| Variable | Address | Type | Role |
| :--- | :--- | :--- | :--- |
| **BPM** | `%I0.2.0` | EBOOL | Start Button |
| **BPA** | `%I0.2.1` | EBOOL | Priority Stop Button |
| **p** | `%I0.2.2` | EBOOL | Presence Sensor |
| **KM1** | `%Q0.3.0` | EBOOL | Main Motor Contactor |
| **KM2** | `%Q0.3.1` | EBOOL | Reserved (Future Directional Control) |

---

### 🛡️ Safety Feature: Deterministic Stop
Unlike a simple software stop, this implementation uses the `INITCHART` function in a high-priority Ladder section. This ensures that the Grafcet state machine is physically reset to Step 0, preventing any "ghost" signals from keeping the motor active during a fault.

---

### 🛠 Tech Stack
* **Software:** Control Expert v15
* **Logic:** SFC (Sequential Function Chart) & LD (Ladder Diagram)
* **Target:** Modicon M340 / M580
