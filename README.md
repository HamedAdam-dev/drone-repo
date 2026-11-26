# ESP-Drone

Open-source quadcopter firmware for Espressif SoCs (Wi-Fi control). This repository is the [espressif/esp-drone](https://github.com/espressif/esp-drone) tree at commit `9d5a4f8`, prepared so you can build and flash it onto an ESP-Drone board.

Flight-control core is derived from [Crazyflie](https://www.bitcraze.io/) and licensed under **GPL-3.0**. See [`LICENSE`](./LICENSE).

## Requirements

- **ESP-IDF**: `release/v5.5` or `release/v6.1` only (enforced by the top-level `CMakeLists.txt`)
- **Default target**: **ESP32-S2** (typical ESP-Drone V1.2 hardware)
- Host tools: Python 3, CMake, Ninja, and the ESP-IDF toolchain for your target

## Quick start (ESP32-S2)

```bash
# From an ESP-IDF environment (after `. $IDF_PATH/export.sh`)
idf.py set-target esp32s2
idf.py build
idf.py -p PORT flash monitor
```

Replace `PORT` with your serial device (for example `/dev/ttyUSB0` or `COM3`). Hold **BOOT** and tap **RESET** if the chip does not enter download mode automatically.

After reboot the drone starts a Wi-Fi SoftAP. Connect a client (ESP-Drone app or CRTP/UDP client) and keep propellers clear until you intentionally arm/fly. See the project flash guide for SoftAP defaults and a first-boot checklist.

## Switch board / SoC target

IDF automatically merges `sdkconfig.defaults` plus the matching `sdkconfig.defaults.<target>` file. Do **not** invent custom pin maps unless your PCB differs from stock ESP-Drone.

| Target | Command | Notes |
|--------|---------|--------|
| ESP32-S2 (default) | `idf.py set-target esp32s2` | Stock ESP-Drone V1.2 profile |
| ESP32-S3 | `idf.py set-target esp32s3` | Same `TARGET_ESP32_S2_DRONE_V1_2` board option |
| ESP32-P4 | `idf.py set-target esp32p4` | Portable/safe + no-radio profile until a real pin map exists |
| ESP32-S31 | `idf.py set-target esp32s31` | Preview; needs IDF 6.1 preview target |

`set-target` recreates `sdkconfig`. Re-apply any local `menuconfig` changes afterward.

Hardware profile and Wi-Fi settings live under **ESPDrone Config** in `idf.py menuconfig` (defaults: SoftAP base SSID `ESP-DRONE`, password `12345678`, channel 6).

## Layout

Upstream ESP-IDF project layout:

- `main/` — entry point and project Kconfig
- `components/` — flight stack, drivers, platform, DSP
- `sdkconfig.defaults*` — shared and per-target defaults
- `LICENSE` — GPL-3.0

## Upstream

- Source: https://github.com/espressif/esp-drone (commit `9d5a4f8`)
- Docs / schematics: [ESP-Drone getting started](https://docs.espressif.com/projects/espressif-esp-drone/zh_CN/latest/gettingstarted.html)
- Apps: [ESP-Drone-iOS](https://github.com/EspressifApps/ESP-Drone-iOS), [ESP-Drone-Android](https://github.com/EspressifApps/ESP-Drone-Android)
