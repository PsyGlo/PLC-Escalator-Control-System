# 🚀 PLC Escalator Control System

**Demand-Driven Escalator with Retriggerable Memory Grafcet**  
*Schneider Control Expert (Unity Pro)*

---

### 📋 Project Description

Implementation of a realistic escalator control system using **Multi-Grafcet** (SFC) structures. The system uses a memory Grafcet (`G_MEMO`) to validate passenger presence with a 15-second retriggerable timer, and a motor control Grafcet (`G_MOTEUR`) for safe start/stop sequences.

---

### 🛠 Tech Stack & Tools

- **Software**: Schneider Control Expert v15 (Unity Pro)
- **Languages**: SFC (Grafcet), Ladder (LD), Structured Text (ST)
- **PLC Target**: Modicon Premium TSX P57 2634M (Simulator)
- **HMI**: EcoStruxure Operator Terminal Expert

---

### 📂 Logic Architecture

| Component          | Language | Role |
|--------------------|----------|------|
| **G_MEMO**         | SFC      | Passenger detection + 15s retriggerable timer |
| **G_MOTEUR**       | SFC      | Motor control (KM1/KM2) |
| **TIMERS**         | Ladder   | TON 15 seconds |
| **Init_Logic**     | Ladder   | Cold start + Emergency reset (INITCHART) |
| **LOGIC_MOTEUR**   | ST       | Safety logic & output management |

---

### 🎯 Key Features

- Safe passenger detection with 15-second confirmation timer
- Emergency stop priority (BPA)
- Proper reset on person leaving or emergency
- Professional HMI supervision screen
- Animation table for debugging

---

### 📹 Demonstration



---

### 🚀 How to Restore

1. Open **Schneider Control Expert**
2. `File > Restore Archive...` → Select the `.sta` file from `/Logic_Source/`
3. Rebuild the project
4. Connect to PLC Simulator → Run
5. Use the Animation Table for testing

---


### 🎓 Academic Context

This implementation is part of the **TSAII** (Technicien Supérieur en Automatique et Informatique Industrielle) curriculum. 
*For detailed credits regarding the AFPA source material, please refer to the [CREDITS.md](./CREDITS.md) file.*
