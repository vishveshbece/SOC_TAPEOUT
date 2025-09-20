# Tool Setup Guide: Yosys, Icarus Verilog, and GTKWave

This guide provides step-by-step instructions to install key **open-source tools** for digital design and verification: **Yosys**, **Icarus Verilog (iverilog)**, and **GTKWave**. These tools form a complete workflow for RTL design, simulation, synthesis, and waveform analysis.

---

## 1. Yosys – RTL Synthesis Tool

**Yosys** is an open-source framework for synthesizing digital circuits described in **Verilog**.  
It converts RTL code into a **gate-level netlist**, which can then be used for simulation, verification, or physical design. Yosys supports many standard cell libraries and can be integrated into larger design flows.

### Installation Steps

```bash
sudo apt-get update
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make
sudo apt-get install build-essential clang bison flex \
  libreadline-dev gawk tcl-dev libffi-dev git \
  graphviz xdot pkg-config python3 libboost-system-dev \
  libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make
sudo make install

!(screenshots/yosy_installation.png)
