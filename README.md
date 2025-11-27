# SEW EURODRIVE: Homing Sequence and PLC Status Monitoring

This repository documents the architecture and initial development of a critical motion control sequence. The project focuses on ensuring a robotic arm system can safely and reliably return to its defined **initial position (Homing)** following a system fault.

## 🛡️ Note on Confidentiality and Project Status

This project is currently in the initial development phase. All implementation details and documentation are **abstracted and de-sensitized** to comply with confidentiality requirements. They serve as a methodology template for integrating legacy PLC systems with modern servo drives.

## ⚙️ Core Architecture and Challenge

The central focus of this project is the integration of proprietary *drive* logic with a legacy control system to facilitate a core motion control function.

* **Key Challenge:** Developing a resilient **"Homing" sequence** to re-establish the zero position of a system (simulando uma robotic arm) after an unplanned shutdown or system error.
* **Drive Logic:** The complex motion sequence (Homing/Reference Run) is programmed directly on the SEW drive using **IPOS Plus**, showcasing competence in decentralized motion control programming.
* **Central Control:** A **discontinued Siemens PLC** acts as the central coordinator.
* **Communication & SCADA:** The PLC-Drive connection is utilized to read the **real-time status** of the SEW Drive. This data is then sent to a **SCADA system** for comprehensive status visualization and diagnostic capabilities. This demonstrates full vertical integration (Drive $\rightarrow$ PLC $\rightarrow$ SCADA).

---

### 1. Drive I/O Configuration and Data Exchange

This section highlights the critical task of mapping physical I/O and configuring the communication channel between the PLC and the SEW Drive.

* **Configured I/O:** Both the **digital inputs and outputs** of the SEW drive were configured and mapped. This included setting up inputs for control commands from the PLC (e.g., enabling, start sequence, fault reset) and outputs for status feedback (e.g., motor ready, homing complete).
* **Safety Interlocks:** Physical inputs were configured for safety devices (e.g., limit switches, safety gate contacts) essential for the IPOS Homing routine.
* **Data Word Mapping:** The primary focus of the PLC-Drive link (SCADA communication) was mapping **Control and Status Words**. This allows the PLC to monitor:
    * **Drive Status:** Ready, Running, Fault Status, Homing Active.
    * **Position Data:** Current motor position data (essential for SCADA visualization).

---

### 2. Motion Control Implementation (IPOS Plus)

* **Function:** The IPOS Plus program is the central brain for finding the mechanical zero of the system.
* **Methodology:** The program defines the specific speed profiles, deceleration ramps, and limit switch handling required to safely execute the Homing sequence.
* **Testing Setup:** For the development phase, a SEW motor with a clamp (*braçadeira*) was used to simulate the robotic arm's movement and positioning feedback.
