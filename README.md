# 🚀 PLC Escalator Control System (Demand-Driven)

This project implements an intelligent escalator control system designed for energy efficiency and passenger safety. Using a **Master/Slave Grafcet architecture**, the system "remembers" when a passenger is on the stairs, ensuring the motor continues to run until they safely reach the top.

---

### 🧠 Logic Architecture (Master/Slave)
The system is divided into two specialized sections to handle concurrent monitoring and execution:

* **G_MEMO (The Memory):** Monitors the presence sensor (`p`). When triggered, it enters a timed state that persists for the duration of the transit (e.g., 15 seconds), effectively "counting" the presence of passengers.
* **G_MOTEUR (The Drive):** Controls the motor contactor (`KM1`). It only activates if the system is started (**BPM**) AND the Memory is active (**X10**). 

> **💡 Technical Note on Retriggerable Logic:** > The use of the `G_MEMO` chart creates a "retriggerable" effect. Even if the sensor `p` only detects a passenger for a split second, the Memory Grafcet ensures the motor remains active long enough for the person to reach the destination. This prevents the motor from "stuttering" or stopping prematurely while a passenger is halfway up.

---

### ⚙️ Input/Output Mapping (Physical PLC)
Mapped for testing on a Schneider Modicon M340/M580 rack:

| Variable | Address | Type | Description |
| :--- | :--- | :--- | :--- |
| **BPM** | `%I0.2.0` | EBOOL | Start Button (Normally Open) |
| **BPA** | `%I0.2.1` | EBOOL | Stop Button (Normally Closed / Safety) |
| **p** | `%I0.2.2` | EBOOL | Presence Sensor (Photo-cell) |
| **KM1** | `%Q0.3.0` | EBOOL | Motor Contactor / Output Indicator |

---

### 🛡️ Safety Implementation
The system includes a **Priority Stop** logic. The motor will immediately cease operation if:
1. The **BPA** (Stop Button) is pressed.
2. An **INITCHART** command is received via the safety task.
3. No passengers have been detected for the duration of the transition timer.

---

### 🛠 Tech Stack
* **Platform:** Schneider Control Expert (Unity Pro)
* **Languages:** SFC (Grafcet) for sequences, LD (Ladder) for safety.
* **PLC Target:** Modicon M340 / M580 Simulator.
