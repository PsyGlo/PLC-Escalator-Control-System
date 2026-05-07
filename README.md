# 🚀 PLC Escalator Control System (Demand-Driven)

This repository contains the implementation of an energy-efficient escalator control system. It uses **concurrent Grafcet sections** to manage passenger detection and motor execution separately.

---

### 🧠 Master/Slave Coordination
The logic is split into two specialized SFC sections within the `MAST` task:

* **Section 1 (G_MEMO):** * Acts as the system "Memory." 
    * When a passenger is detected (`p`), it moves to step **S_1_2**.
    * It uses the transition syntax `t#15s / S_1_2.X` to ensure the system remembers the passenger for exactly 15 seconds.
* **Section 2 (G_MOTEUR):** * Acts as the "Drive Controller."
    * It monitors the activity bit of the memory step (**S_1_2.X**).
    * **Start Condition:** `BPM AND S_1_2.X`
    * **Stop Condition:** `BPA OR NOT S_1_2.X`

> **💡 Technical Insight:** > By using the `.X` activity bit of a step from a different section, we achieve **deterministic synchronization**. This ensures that the motor can never run unless the memory timer is active, providing a fail-safe relationship between sensors and actuators.

---

### ⚙️ I/O Configuration
Mapped for the Schneider Modicon M340/M580 PLC:

| Variable | Address | Type | Role |
| :--- | :--- | :--- | :--- |
| **BPM** | `%I0.2.0` | EBOOL | Start Button |
| **BPA** | `%I0.2.1` | EBOOL | Stop Button (Priority) |
| **p** | `%I0.2.2` | EBOOL | Presence Sensor |
| **KM1** | `%Q0.3.0` | EBOOL | Motor Output (Step Action: N) |

---

### 🛠 How to Restore
1. Open Schneider Control Expert.
2. Go to **File > Restore Archive** and select the `.sta` file.
3. Build the project and transfer it to the **PLC Simulator**.
4. Use the **Animation Table** to toggle `p` and watch the 15s countdown in the `G_MEMO` transition.
