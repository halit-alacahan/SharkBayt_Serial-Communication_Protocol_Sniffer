# 🦈 SharkBayt: Autonomous Serial Communication Protocol Sniffer & Identifier

SharkBayt is an embedded hardware diagnostic tool and black-box sniffer designed to autonomously probe unknown target test points, identify active serial communication protocols (UART, SPI, I2C, JTAG), and capture live packet traffic.

---

<p align="center">
  <img src="Bitirme%20PCB.png" alt="SharkBayt PCB Hardware Preview" width="750">
</p>

---

## 📌 Project Overview

Reverse-engineering unknown target headers or undocumented embedded interfaces typically requires manual probing with logic analyzers and oscilloscopes. **SharkBayt** automates this discovery phase using a 5-channel multiplexing matrix coupled with a high-performance ARM Cortex-M4 microcontroller. 

It systematically scans connected probe lines (P1–P8), analyzes clock transitions, idle line states, baud rates, and frame topologies, identifies the protocol on the fly, and routes traffic back to a host computer over a high-speed USB interface.

---

## 🛠️ Hardware Architecture & Specifications

### 1. Main Processing Core
* **MCU:** **STM32F446VET6** (ARM Cortex-M4 @ 180 MHz, DSP, FPU, 512 KB Flash, 128 KB SRAM).
* **System Clocks:** Dedicated 8 MHz high-speed external oscillator (HSE) and 32.768 kHz RTC oscillator (LSE).
* **Debugging & Control:** Standard SWD programming connector, user Wake-up/Reset switches, and Boot mode jumpers.

### 2. Analog Multiplexer Matrix (Target Interfacing)
* **Multiplexers:** 5x **74HC4051D** 8-channel analog multiplexer/demultiplexer ICs (`U2` to `U6`).
* **Probe Inputs:** 8 individual probe test points (`P1` through `P8`) interfaced through dedicated headers (`J1`, `J4`).
* **Dynamic Routing:** Each multiplexer selects one of the 8 probe lines and maps it directly to the internal hardware peripherals (SPI, I2C, USART, EXTI) of the STM32 MCU (`MOUT1`–`MOUT5`).
* **Addressing & Enable Control:** Dedicated GPIO select lines (`S0`, `S1`, `S2`) and independent enable control (`EN`) for each individual multiplexer stage.

### 3. Protocol Detection & Visual Status Indicators
* **Supported Protocols:** Autonomous detection and decoding of **SPI**, **I2C**, **UART**, and **JTAG** interfaces.
* **Hardware Protocol Indicators:** Dedicated diagnostic LEDs for instant visual feedback:
  * 🟢 **SPI Active LED** (Green)
  * 🟡 **I2C Active LED** (Yellow)
  * 🔴 **UART Active LED** (Red)
  * 🟠 **JTAG Active LED** (Amber)

### 4. Host PC Interface & Connectivity
* **USB-to-UART Bridge:** **FTDI FT232RNL** via Mini-USB connector (`J3`), featuring dedicated ferrite bead filtering and ESD/noise decoupling.
* **Expansion Headers:** Onboard breakout pins for external probing, I2C routing, and direct hardware access.

### 5. Power Architecture & Voltage Regulation
* **Dual Power Input:**
  * **External DC Input:** 12V main power input via terminal header (`J5`).
  * **USB Power Input:** 5V `VBUS` input from host PC.
* **Step-Down Regulation (12V to 5V):** High-efficiency synchronous buck converter (`U8`) generating the primary `5V_SYS` rail.
* **Linear Regulation (5V to 3.3V):** Dedicated LDO regulator (`U9`) powering the MCU core, multiplexers, and FTDI logic.
* **Power Path & Protection:** MOSFET-driven auto-switching and power path management between USB and external DC inputs, paired with PPTC fuse protection.

---

## 👥 Authors & Contributors

* **Hardware Design & Engineering:** Halit ALACAHAN
* **Embedded Software & Firmware Development:** Yahya ÇAKIR

### Academic Advisors & Contributors
* **Prof. Dr. Yasin ÖZÇELEP** (Advisor)
* **Dr. Öğr. Üyesi Mehmet Yavuz YAĞCI**
* **Arş. Gör. Mustafa ŞİRİN**

---
## 📜 License & Proprietary Notice

Copyright © 2025 **[Takım/Şirket Adı]**. All rights reserved.

The hardware schematics, PCB designs, and related files in this repository are proprietary. No license is granted for commercial use, mass production, modification, or redistribution without prior written consent.

## 📁 Repository Structure

```text
├── Altium PROJECT/          # Altium Designer project files (.PrjPcb, .PcbDoc)
├── Altium SCHEMATIC/        # Modular schematic sheets (MCU, MUX, Power)
├── Bitirme PCB.png          # Top 3D hardware render / board preview
├── License.md               # License terms and usage rights
└── README.md                # Project documentation and architecture overview
