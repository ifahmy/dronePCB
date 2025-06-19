
# Tiny Drone Flight Controller PCB

This repository documents the design of a custom STM32-based nano drone flight controller, incorporating power management, 2.4 GHz radio, motion sensors, and brushed motor drivers. Designed for compatibility with **ArduPilot** and **Betaflight**.

---

## Microcontroller Unit (MCU)

- **STM32F722RET6**, selected for performance and documentation.
- 8 MHz HSE oscillator with tuning capacitors for STM32 and sensors.
- Power monitoring via voltage divider and ADC.
- On-board **SWDIO/SWCLK** for debugging and bootloader flashing (DFU mode via BOOT and RESET).
- **Decoupling capacitors** used throughout for stability under current spikes.

---

## MCU Pin Assignments

- Verified via STM32CubeIDE.
- PWM signals generated via hardware timers.
- Deconflicted JTAG and other peripheral capabilities.
- SPI used for IMU (vs I2C in Nano).
- Pin selections support both ArduPilot and Betaflight configurations.

---

## Sensors & Motor Drivers

- **IMU**: ICM-42688-P (connected via SPI3 for efficiency).
- **Barometer**: MS5611 (lower data rate, connected via I2C with pull-ups).
- **Motors**: Driven by SI2302 N-Channel MOSFETs (supports high-frequency PWM, no gate driver required).
- **Flyback protection**: SS14 Schottky diodes.
- **Gate protection**: 10Ω resistors to suppress transients.

---

## Power Regulation & Charging

- **Battery input** with reverse polarity protection.
- **MCP73831-2** for LiPo charging via USB (charge current ~40 mA with 24.9k resistor).
- **MCP1700** LDO regulator (2.8V output) for analog and digital circuits.
- **Ideal diode configuration** using PMOS + Schottky for automatic switching between USB and battery.

---

## 2.4 GHz Radio & USB Interface

- **nRF24L01+** radio connected to SPI1 with CE and CS via GPIO.
- External ¼-wave wire antenna (~1.25”) with matching network based on datasheet.
- **16 MHz oscillator** for the radio.
- **Micro-USB** interface with 27Ω series resistors.
- **PRTR5V0U2X** for USB ESD protection.

---

## PCB Design Overview

- 4-layer board:
  - Top and Bottom: Signal layers
  - Inner Layers: +2.8V and Ground
- Motor PWM routed in +2.8V layer due to limited space.

---

## Next Steps & Recommendations

- Order fabricated PCBs.
- Flash bootloader / firmware using DFU for Betaflight/ArduPilot.

### Future Improvements
- Migrate from 4-layer to 2-layer for cost/manufacturing optimization.
- Reroute motor gate PWM signals away from power planes if space allows.

---

## Bill of Materials (BOM)

For a full list, see the Bill of Materials Excel sheet. Sample entries:

| Ref | Description | Value | Manufacturer Part # | Digi-Key Part # |
|-----|-------------|--------|----------------------|-----------------|
| U1  | IMU         | ICM-42688-P | ICM-42688-P | 1428-ICM-42688-PTR-ND |
| U2  | Barometer   | MS5611-01BA | MS561101BA03-50 | 223-1622-1-ND |
| U3  | RF Radio    | nRF24L01+   | NRF24L01P-T | 4823-NRF24L01P-T-ND |

---

## Tools Used

- STM32CubeIDE (Pin config, firmware)
- KiCad 9.0  (PCB layout and schematic)
- DigiKey (Component sourcing)
- Calipers (Nano board dimensions)

