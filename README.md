# 🛰️ Cellular GPS Asset Tracker: System Overview & Custom Hardware Evolution

This repository serves as the technical documentation and code for an end-to-end, intelligent IoT asset tracking solution. The project demonstrates an entire industrial IoT data path: from bare-metal hardware acquisition and cellular transmission to database storage and web visualization.

---

## 🚀 Project Status: Prototype Success to Custom Hardware Evolution

This project is actively moving from a successful **Proof-of-Concept (PoC)** phase into a **Next-Generation Custom Hardware** architecture.

* **Phase 1: Full-Stack Proof of Concept (Completed):** A complete end-to-end working system was engineered using an STM32F407, a **SIM7600E** cellular shield, a custom bare-metal C client, and a dedicated C backend server.
* **Phase 2: Custom Hardware Migration (In Progress):** To transition the system into a compact, low-power industrial product, the architecture is being migrated to a fully custom-designed PCB. This replaces the bulky development boards with a highly optimized, integrated circuit layout.

---

## 🛠️ Next-Generation Custom Hardware Architecture (WIP)

The upcoming hardware iteration (detailed in the `hardware/` folder) transitions the project to an ultra-efficient, production-ready design.

### Key System Components
* **Core Modem:** Integrated **ST87M01-1301** module, leveraging its advanced industrial NB-IoT/LTE-M connectivity and deep sleep power states.
* **Host MCU:** **STM32G0B0CET6** microcontroller handling sensor aggregation, AT-command scheduling, and power state orchestration. This choice provides massive power savings and footprint reduction over the prototyping board while maintaining rich peripheral support (UARTS, SPI, I2C).
* **Li-Ion Cell Charge Management:** Onboard lithium-ion charging circuitry, enabling reliable field deployment and solar/USB harvesting configurations.
* **Cell Protection:** Integrated hardware protection circuits safeguarding the battery against over-voltage, under-voltage, and over-current conditions.
* **Power Rails:** Optimized multi-rail power distribution network designed to completely isolate and cut power to non-essential sub-systems during sleep cycles to achieve micro-amp quiescent draw.

### Schematic Architecture Hierarchy:
The new hardware design is structured hierarchically in KiCad. You can view the full multi-sheet layout here: **[View / Download the Schematic PDF](./hardware/ST87M01_Tracker_Board.pdf)**
* **Host_MCU:** The brain of the tracker, centering around the STM32G0B0CET6 to manage low-level peripherals and state machines.
* **NB_IoT_Modem_p1:** The RF and network interface layer built around the ST87M01-1301.
* **Li-Ion Cell Charge Management:** Dedicated charging paths designed for battery safety and longevity.
* **Power Rails:** Segmented voltage regulation to maximize energy efficiency.

---

## ⚙️ Architectural Components (PoC Baseline)

The existing, validated tracking architecture is split into three distinct, communicating layers:

### 1. Embedded Firmware (The Client)

The tracking logic runs on the STM32F407 and is responsible for hardware initialization, data collection, and robust communication protocols.

* **Platform:** STM32F407VETx Development Board.
* **Core Implementation:** All software is written **from scratch in bare-metal C**, focusing on efficiency and deterministic execution. **Low-level drivers** (e.g., UART, GPIO) are implemented based on principles and information provided in the following resource: [PacktPublishing/Bare-Metal-Embedded-C-Programming](https://github.com/PacktPublishing/Bare-Metal-Embedded-C-Programming).
* **GPS Acquisition:** Logic handles the SIM7600E AT commands for GPS fix retrieval and state monitoring (including LED indicators for Initialization and Transmission Errors).
* **Transmission Logic:** Implements HTTP POST requests to package collected GPS data and send it to the external server.

### 2. Hardware Prototype (The Tracker Board)

* **Microcontroller:** STM32F407-based Development Board.
* **Connectivity:** **SIM7600E** LTE Cat-1 Module Board for cellular communication (4G data transmission).
* **Power Supply:** Uses a standard USB Power Bank for portable, real-world testing.
* **Status Interface:** Dual LED status indicators provide immediate visual diagnostics, signaling:
    * `ERROR_INIT` (Hardware/Configuration Failure)
    * `ERROR_RUNTIME` (Data/Processing Failure)
    * `ERROR_TX` (Critical Communications Failure)
  
![Tracker Board Hardware Setup](docs/tracker_board_hw_prototype.jpg)

### 3. Data Server & Visualization (The Backend)

The server component, developed in C, is the data ingestion and display endpoint for the tracking system.

* **Server Repository:** [AndreasCnaus/simple\_http\_server: HTTP server that receives GPS data via POST requests and stores it in an SQLite database](https://github.com/AndreasCnaus/simple_http_server)
* **Data Ingestion:** The server receives raw tracking packets from the STM32F4 client via **HTTP POST requests**.
* **Data Storage:** Received GPS coordinates and timestamps are securely saved in an **SQLite database**.
* **Visualization:** Data queried from the database is rendered into dynamic maps and plots using the **Plotly** library, enabling real-time analysis of the asset's movement history.

![GPS Track Data for 18.11.2025](docs/test_gps_track_18_11_2025.png)

---
