# JOURNAL.md

## 2026-08-20 - Literature Review
Read 2022 JAMIA Open paper: GBDT with HR + Temp achieves 0.94 AUROC on ICU data. Key insight: 2 vitals alone can be powerful. PULSE adds SpO2 and constrains for edge deployment - novel gap.

## 2026-08-22 - Sensor Selection
Finalized MAX30102 for SpO2/HR and MLX90614 for temp. Both I2C, both proven, both low power. Contactless temp avoids infection control issues. BP deferred to manual entry for V1.

## 2026-08-24 - Architecture Lock
Settled on ESP32-S3 over RP2040. 512KB SRAM gives headroom for 6-hour buffer + 35KB model + display. WiFi built-in for local nurse station alerts. Architecture document finalized.

## 2026-08-24 - Funding Application
Drafted README for Hack Club Highway. Target: $250-350 for 3 prototype units + V2 BP components.
