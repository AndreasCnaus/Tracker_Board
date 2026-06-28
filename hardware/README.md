# 📂 Tracker_Board: Hardware Section

⚠️ **DISCLAIMER: WORK IN PROGRESS & UNTESTED DESIGN**

The schematics and hardware files contained in this directory are actively under development and are currently **untested**. 

Because this design includes **Lithium-Ion (Li-Ion) Cell Charge Management and Protection circuits**, improper implementation, assembly, or modification carries inherent risks—including short circuits, thermal runaway, fire, or cell explosion. 

* **Use at your own risk:** Any manufacturing, testing, or usage of these hardware designs is entirely at your own discretion.
* **No Liability:** The author assumes absolutely no responsibility or liability for hardware failures, component damage, physical injury, or any property damage resulting from the use of these files.

---

## 🗺️ Hardware Contents

* **`ST87M01_Tracker_Board.pdf`**: Full hierarchical multi-sheet design containing:
  * **Host MCU:** **STM32G0B0CET6** microcontroller core handling state machine execution and peripheral scheduling.
  * **Modem Interface:** **ST87M01-1301** industrial NB-IoT/LTE-M module integration.
  * **Motion Sensing:** **LIS3DH** 3-axis accelerometer for hardware-interrupt-driven motion detection and wake-up logic.
  * **Power Subsystem:** High-efficiency **TPS63020** Buck-Boost converter, low-noise **TLV75725** LDO regulator, and integrated cell management/protection circuitry.