# Week 2 – Task 1: Exploring SoC Design with VSDBabySoC

This task documents my progress studying **System on Chip (SoC) design**, focusing on **VSDBabySoC** — a small, open-source SoC based on the RISC-V architecture. The task helped me explore how digital components, analog interfaces, and synchronization mechanisms combine to create a working SoC.

---

## 🚀 Learning the Basics of SoC Design

### What is a System on Chip (SoC)?
A SoC is a complete computing system integrated on a single chip. It typically includes:

- Processor (CPU)  
- Memory  
- Input/Output (I/O) interfaces  
- Analog components (sometimes)  

SoCs are compact, efficient, and low-power, ideal for smartphones, IoT devices, wearables, and embedded systems.

### Types of SoCs
- **Microcontroller-based SoC:** Low power, used in home appliances, cars, and simple IoT devices.  
- **Microprocessor-based SoC:** Runs operating systems, used in smartphones/tablets.  
- **Application-Specific SoC (ASIC):** High-performance tasks like graphics, AI, or networking.

### Key Parts of an SoC
- **Processor (CPU):** Executes instructions and runs programs.  
- **Memory:** RAM (temporary) and flash (permanent).  
- **I/O Interfaces:** Connect the SoC to sensors, displays, networks.  
- **Analog Components:** DACs and ADCs for real-world signals.  
- **Clock Generation (PLL):** Synchronizes all components.

### Importance of SoCs
- Compact design  
- Energy efficient  
- High performance  
- Cost effective  
- Reliable

---

## 👶 Introduction to VSDBabySoC

**VSDBabySoC** is a minimal RISC-V SoC designed for educational and testing purposes.

### Core Components
- **RVMYTH CPU (RISC-V Core):** Executes instructions, uses register `r17` for DAC output.  
- **PLL (Phase-Locked Loop):** Generates stable clock signals.  
- **DAC (Digital-to-Analog Converter):** 10-bit DAC converts digital CPU values into analog signals.

### How VSDBabySoC Works
1. **Initialization:** Receives input, PLL generates a stable clock.  
2. **Data Processing:** RVMYTH CPU processes data in `r17`.  
3. **Analog Output:** DAC converts digital values to analog; signals are saved to `OUT` or sent to devices.

### DAC — Digital to Analog Converter
- **Purpose:** Convert binary digital values into smooth analog voltages.  
- **Resolution:** 10-bit (1024 levels).  
- **Types:** Weighted resistor DAC, R-2R ladder DAC.  

---

## 💡 Key Learnings
- Integration of RISC-V CPU and basic processor design.  
- Clock synchronization using PLLs.  
- Digital-to-analog data flow and peripheral integration.  
- SoC architecture planning and RTL hardware description.  
- Real-world application of small SoCs in larger systems.

---

## 📚 Conclusion
VSDBabySoC is a **complete learning platform** connecting digital logic, analog interfaces, and real-world system design. This task gave me a deeper understanding of **SoC architecture** and the **digital-to-analog pipeline** that powers modern electronics.
