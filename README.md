# STM32 & SD NAND Flash Storage Reference Design

This repository provides an open-source hardware layout guide and firmware implementation example for integrating **LGA-8 SD NAND Flash** (CS創世 SD NAND) into **STM32/ESP32** embedded systems.

Designed for industrial IoT edge devices, dataloggers, and embedded systems requiring high reliability, resistance to vibration, and small footprint.

---

## 💡 Why SD NAND over Traditional MicroSD / TF Cards?

In industrial environments, traditional MicroSD cards often present failure points:
- **Mechanical Instability:** Card socket pins wear out or disconnect due to continuous vibration.
- **Supply Chain & Longevity:** Consumer-grade TF cards change controllers and Flash memory chips without notice.
- **Physical Footprint:** Card sockets occupy significant PCB space.

### Key Advantages of LGA-8 SD NAND:
- **Solderable LGA-8 Package:** Eliminates mechanical connectors and slot failures.
- **Built-in Controller & Wear Leveling:** Operates via standard SDIO / SPI protocol just like a standard SD card.
- **Industrial Grade Reliability:** Supports wide operating temperatures (-40°C to +85°C) and extended P/E cycles.
- **Compact Size:** Fits easily in space-constrained IoT edge devices.

---

## 🛠 Hardware Connection (SPI / SDIO Interface)

SD NAND communicates seamlessly via standard **SDIO** (4-bit/1-bit) or **SPI** bus interfaces. Below is a standard 1-bit SPI pinout reference for MCU integration:

| SD NAND Pin (LGA-8) | Signal Name | MCU Connection (e.g. STM32) |
| :--- | :--- | :--- |
| **Pin 1** | DAT2 | NC / Pull-up |
| **Pin 2** | CD/DAT3 (CS) | SPI_CS (Chip Select) |
| **Pin 3** | CMD (DI) | SPI_MOSI / SDIO_CMD |
| **Pin 4** | VSS | GND |
| **Pin 5** | VDD | 3.3V Power (Add 0.1uF Decoupling Capacitor) |
| **Pin 6** | CLK | SPI_SCK / SDIO_CK |
| **Pin 7** | VSS | GND |
| **Pin 8** | DAT0 (DO) | SPI_MISO / SDIO_DAT0 |

> **Layout Tip:** Place a 10uF bypass capacitor and a 0.1uF ceramic decoupling capacitor as close to VDD (Pin 5) as possible to stabilize voltage during high-current write operations.

---

## 💻 Software & Driver Support

Since SD NAND integrates a full SD controller onboard, **no custom low-level NAND driver or complex FTL (Flash Translation Layer) software is required**.

You can use standard embedded file systems directly out of the box:
- **FatFs** (Standard FAT16/FAT32 File System Library)
- **LittleFS** (Optimized for power-fail safe operation)
- **FreeRTOS / ThreadX** integrated storage stack

---

## 📁 Repository Structure

```text
├── Hardware/
│   ├── Schematic_Reference.pdf     # Reference schematic snippets
│   └── KiCad_Footprint/            # LGA-8 Footprint for KiCad
├── Examples/
│   ├── STM32_SPI_FatFs_Example/    # Basic FatFs read/write demo
│   └── ESP32_SDIO_Logger/          # High-speed data logging code
└── README.md                       # Documentation
<img width="1494" height="1320" alt="3ae33921-cc36-475d-b669-94469290c1f1" src="https://github.com/user-attachments/assets/f8386e9b-6eda-48f1-b76d-c17364ba580e" />
<img width="1557" height="1307" alt="2e676097-0653-443a-882b-b64d717dfce9" src="https://github.com/user-attachments/assets/85d3a28b-aafd-49bc-8ddb-209e14552360" />
