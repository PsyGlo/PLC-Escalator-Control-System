# 🚀 PLC Escalator Control System (Demand-Driven)

This project demonstrates an industrial escalator control system developed in **Schneider Control Expert (Unity Pro)**. It uses a Master/Slave Grafcet architecture to ensure the motor only runs when passengers are present and safely clears the stairs before stopping.

---

### 🧠 Master/Slave Logic (The "Memory" Link)
The project is split into two functional SFC sections. Because the software uses hierarchical naming, the coordination is handled via **Step Activity Bits**:

* **G_MEMO (Master - Section 1):** * Initial State: `S_1_1`
    * Memory/Timer State: `S_1_2` (Triggered by presence sensor `p`).
* **G_MOTEUR (Slave - Section 2):**
    * Uses the condition `S_1_2.X` to determine if the motor should be running.
    * This ensures the motor stays active for the full duration of the timer defined in the Master chart.

> **💡 Technical Note on Step Naming:** > In this implementation, we utilize the `.X` suffix (e.g., `S_1_2.X`). This allows for deterministic communication between separate SFC sections without the need for intermediate boolean variables, reducing memory overhead and scan-time latency.

---

### ⚙️ I/O Mapping & Hardware Configuration
Targeted for a **Schneider Modicon M340/M580** rack:

| Variable | Address | Type | Description |
| :--- | :--- | :--- | :--- |
| **BPM** | `%I0.2.0` | EBOOL | Start Button (Bouton Marche) |
| **BPA** | `%I0.2.1` | EBOOL | Stop Button (Bouton Arrêt - Safety) |
| **p** | `%I0.2.2` | EBOOL | Presence Sensor (Sensor) |
| **KM1** | `%Q0.3.0` | EBOOL | Motor Contactor (Qualitative N) |

---

### 🛠 How to Test
1. Restore the `.sta` file in Control Expert.
2. Observe the `G_MOTEUR` transitions. You will see they are linked directly to the status of `S_1_2` in the `G_MEMO` section.
3. Use the **Animation Table** to force the `p` sensor and watch the timer count down in `G_MEMO`.
