# 🚀 PLC Escalator Control System (Demand-Driven)

This project demonstrates a high-reliability escalator control solution designed for Schneider Electric PLCs (Modicon M340/M580). It utilizes a multi-language approach (SFC + Ladder) to balance energy efficiency, retriggerable timing, and hardware safety.

---

### 🧠 System Architecture

The project is modularized into three distinct logic sections within the `MAST` task:

1. **G_MEMO (SFC - Master):** - Handles passenger detection.
   - Transitions to step **S_1_2** upon sensor trigger (`p`).
   - Implements a 15-second retriggerable timer: `t#15s / S_1_2.X`.

2. **G_MOTEUR (SFC - Slave):**
   - Controls the physical motor contactor (**KM1**).
   - Logic is slave to the memory bit: 
     - *Start:* `BPM AND S_1_2.X`
     - *Stop:* `BPA OR NOT S_1_2.X`

3. **Init_Logic (Ladder - Safety):**
   - Uses the **INITCHART** function.
   - Automatically resets all sequences on Cold Start (`%S1`) or when the Emergency Stop (**BPA**) is engaged.

---

### ⚙️ Hardware I/O Mapping

| Variable | Address | Type | Role |
| :--- | :--- | :--- | :--- |
| **BPM** | `%I0.2.0` | EBOOL | Start Push Button (NO) |
| **BPA** | `%I0.2.1` | EBOOL | Stop Push Button (NC / Safety) |
| **p** | `%I0.2.2` | EBOOL | Presence Sensor (Photo-eye) |
| **KM1** | `%Q0.3.0` | EBOOL | Motor Contactor - Direction 1 |
| **KM2** | `%Q0.3.1` | EBOOL | Motor Contactor - Reserved (Direction 2) |

---

### 🛡️ Safety & Determinism
By linking the Drive logic directly to the Memory step bit (`S_1_2.X`), the system ensures that the escalator never stops while a passenger is mid-transit. The inclusion of Ladder-based initialization ensures that the state machine cannot get "stuck" in an active state after a power failure or emergency stop.

---

### 🛠 Tech Stack
* **Software:** Schneider Control Expert v15 (Unity Pro)
* **PLC Target:** Modicon M340 / M580
* **Languages:** SFC (Grafcet) and LD (Ladder Diagram)
