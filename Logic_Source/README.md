📂 Logic_Source

This folder contains the primary automation files for the **Escalier Mécanique** (Escalator Control System) project. The logic is designed to demonstrate Multi-Grafcet coordination with memory-based passenger detection and safety priority management.

---

📄 File Inventory

- `Escalier_Mecanique.stu`: The primary project archive. This is the most reliable file for opening the project in Schneider Control Expert.
- `Escalier_Mecanique.zef`: A full project export used for comprehensive backups.
- `Escalier_Mecanique.xef`: An XML-based export. This version allows for text-based version tracking of the Grafcet logic and variable definitions.

---

⚙️ Technical Highlights

**Architecture**: Uses a Multi-Grafcet approach with two coordinated charts:
- `G_MEMO`: Memory Grafcet responsible for passenger presence detection and 15-second retriggerable timer.
- `G_MOTEUR`: Motor control Grafcet managing KM1/KM2 contactors with emergency stop priority.

**Functions Implemented**:
- INITCHART: High-priority initialization and manual reset of both Grafcets.
- TON Timer: 15-second presence validation with retrigger capability.
- Structured Text: Safety logic and output management in LOGIC_MOTEUR.

**Data Types**: Extensive use of EBOOL for diagnostic forcing and history tracking in the animation table.

---

🚀 Usage

To run this project:

1. Open Schneider Control Expert.
2. Go to **File > Restore Archive...** and select the `.stu` file from this folder.
3. Once opened, go to **Build > Rebuild All Project**.
4. Connect to the **PLC Simulator**, transfer the project, and set to **Run**.
5. Use the **Animation Table** to observe the interaction between G_MEMO and G_MOTEUR.

---

**Made with determination ❤️**
