# Voron 2.4 Formbot Reference & Configuration Guide

---

## Table of Contents
- [1. Hardware List & Bill of Materials](#1-hardware-list--bill-of-materials)
  - [Core Control & Power Electronics](#core-control--power-electronics)
  - [Toolhead, Motion & Probing](#toolhead-motion--probing)
  - [Thermal, Filtration & Sensors](#thermal-filtration--sensors)
  - [Structural, Panels & Build Volume](#structural-panels--build-volume)
- [2. Assembly Guides & Flashing Documentation](#2-assembly-guides--flashing-documentation)
  - [Mechanical & Frame Build Manuals](#mechanical--frame-build-manuals)
  - [Firmware, Flashing & Board Configuration](#firmware-flashing--board-configuration)
  - [Klipper & System Configuration](#klipper--system-configuration)
- [3. Overview of `printer.cfg`](#3-overview-of-printercfg)
- [4. Configuration Modular Breakdown](#4-configuration-modular-breakdown)
- [5. Klipper Documentation & Resources](#5-klipper-documentation--resources)

---

## 1. Hardware List & Bill of Materials

### Core Control & Power Electronics
| Component | Qty | Notes & Reference Links |
| :--- | :---: | :--- |
| **BTT Manta M8P + CB1** | 1 | Main control board and Compute Module host. [(Manta M8P Repo)](https://github.com/bigtreetech/Manta-M8P) |
| **BTT EBB SB2209 CAN (RP2040)** | 1 | Toolhead expansion board. [(BTT Wiki)](https://global.bttwiki.com/EBB%202209%20CAN%20RP2040.html) \| [(BTT EBB Repo)](https://github.com/bigtreetech/EBB) |
| **TMC2209 Stepper Drivers** | 6 | Stepper drivers (X, Y, Z0, Z1, Z2, Z3). [(BTT Wiki)](https://global.bttwiki.com/TMC2209.html) \| [(BTT Driver Repo)](https://github.com/bigtreetech/BIGTREETECH-Stepper-Motor-Driver) |
| **BTT HDMI5 Touch Screen** | 1 | 5" 800x480 Capacitive IPS display. [(BTT HDMI5 Specs)](https://github.com/bigtreetech/docs/blob/master/docs/HDMI5.md) \| [(TouchScreen Hardware Repo)](https://github.com/bigtreetech/BIGTREETECH-TouchScreenHardware) |
| **MEAN WELL LRS-200-24** | 1 | 24V 200W main system power supply. [(Data Sheet)](https://meanwell-ps.com/products/lrs-200-24) |
| **Omron G3NB-210B-1** | 1 | Solid State Relay for AC bed heater. [(Omron Specs)](https://www.omron-ap.com/products/family/3232/specification.html) |
| **CW2A-10A-T Power Inlet** | 1 | Filtered power entry module with switch/fuse socket. |
| **Medium Blow Fuses (5x20mm)** | 4 | 2x 4A (220V) / 2x 8A (120V) mains protection. |

### Toolhead, Motion & Probing
| Component | Qty | Notes & Reference Links |
| :--- | :---: | :--- |
| **Voron TAP V2 PCB & Kit** | 1 | Nozzle-based optical Z-probe system. [(Voron TAP GitHub)](https://github.com/VoronDesign/Voron-Tap) |
| **Hotend Kit** | 1 | Complete hotend assembly with heat block. |
| **PT1000 Temperature Sensor** | 1 | 2-wire RTD probe (1000Ω at 0°C); uses native 4.7kΩ pull-up resistor on BTT board. |
| **24V 70W Heater Cartridge** | 1 | High-output heating head (Ø6x20mm, up to 500°C). [(Heater Specs)](https://www.hotend.eu/p/heater-70w-24v) |
| **MOONS' NEMA17 Steppers** | 6 | `17HS19-2004S1-H` (1.8°, 2.0A/Phase, 59 N·cm, Class H 180°C). [(Stepper Datasheet)](https://au-stepperonline.com/index.php?route=product/product/get_file&file=1557/17HS19-2004S1-H_Full_Datasheet.pdf) |
| **MOONS' NEMA14 Pancake Motor**| 1 | `CSE14HRA1L410A` 36mm round motor w/ 10T gear for CW2 extruder (1.8°, 1.0A). [(Pancake Datasheet)](https://cdn.shopify.com/s/files/1/0541/6638/8905/files/CSE14HRA1L410A-01.pdf?v=1698644077) |
| **MGN9H / MGN12H Linear Rails** | 8 | 6x MGN9H, 1x MGN12H, 1x 50mm MGN9H rail block. |
| **GT2 Motion Belts & Loops** | -- | LL-2GT-9 (9mm), LL-2GT-6 (6mm open), & 188mm loop belts. |
| **Bearings & Precision Shafts** | -- | 20x F695 bearings, 12x 625 bearings, 4x Ø5x60mm D-cut shafts. |

### Thermal, Filtration & Sensors
| Component | Qty | Notes & Reference Links |
| :--- | :---: | :--- |
| **Silicone AC Heater Pad** | 1 | Mains-voltage bed heater with integrated thermistor & 125°C thermal fuse. |
| **Nevermore Carbon Filter Kit** | 1 | Air filtration element, active carbon pack, and WAGO 221-413 connectors. |
| **Filament Runout Sensor** | 1 | Switch sensor connected to `EBBCan:gpio21`. |
| **5050 / 4010 / 6020 Fans** | 7 | 3x 5015 centrifugal, 1x 4010 axial, 3x 6020 enclosure fans (24V). |
| **Stealthburner & Case LEDs** | -- | Addressable Neopixel toolhead harness & chamber lighting strips. |

### Structural, Panels & Build Volume
| Component | Qty | Notes & Reference Links |
| :--- | :---: | :--- |
| **Misumi 2020 Extrusions** | 18 | Pre-cut & tapped aluminum frame extrusions (HFSB5 series). |
| **Cast Aluminum Plate 5/16"** | 1 | Precision machined cast bed plate. |
| **Flexible Spring Steel Surface** | 1 | PEI print sheet with adhesive magnetic backing base. |
| **Enclosure Panels** | 8 | 2x bottom black, 1x back black, 2x front clear, 2x side clear, 1x top clear. |
| **DIN Rails & Brackets** | 2 | 35mm x 365mm DIN rails with SSR mounting bracket & end caps. |

---

## 2. Assembly Guides & Flashing Documentation

### Mechanical & Frame Build Manuals
| Guide / Manual | Description | Link |
| :--- | :--- | :--- |
| **Voron 2.4 R2 Assembly Manual** | Official core mechanical manual for frame, motion system, and gantry setup. | [(Voron 2.4 Manual PDF)](https://raw.githubusercontent.com/VoronDesign/Voron-2/Voron2.4/Manual/Assembly_Manual_2.4r2.pdf) |
| **Formbot's Build Notes** | Build notes from Formbot | [(Build Notes)](https://github.com/FORMBOT/Voron-2.4/tree/main/Build%20Notes)
| **Voron "Wish I Knew" guide** | User build guide for the Formbot Voron kit | [(User Build guide)](https://github.com/Zev-se/Formbot-voron-2.4-build-guide/blob/main/guide.md) |
| **Voron Stealthburner Manual** | Toolhead, Clockwork2 extruder, and Neopixel assembly guide. | [(Stealthburner Manual PDF)](https://github.com/VoronDesign/Voron-Stealthburner/blob/main/Manual/Assembly_Manual_SB.pdf) |
| **Voron TAP Manual** | Optical probe assembly and toolhead mount integration. | [(Voron TAP Manual PDF)](https://github.com/VoronDesign/Voron-Tap/blob/main/Manual/Assembly_Manual_Tap.pdf) |
| **Manta M8P v2 Wiring** | Wiring diagram and routing for Manta M8P, PSU, and SSR. | [(Formbot V2.4 Pro Wiring)](https://docs.vorondesign.com/build/electrical/v2_m8p_wiring.html) |

### Firmware, Flashing & Board Configuration
| Hardware / System | Scope | Reference & Guide Links |
| :--- | :--- | :--- |
| **CB1 OS Image & Flashing** | Burning Linux OS image to MicroSD for the BTT CB1 compute module. | [(BTT CB1 OS Manual)](https://github.com/bigtreetech/CB1) \| [(BTT Docs)](https://global.bttwiki.com/CB1.html) |
| **Manta M8P Klipper Firmware** | Compiling and flashing Klipper firmware via SD card / DFU. | [(Manta M8P User Manual)](https://github.com/bigtreetech/Manta-M8P/blob/master/V2.0/BIGTREETECH%20MANTA%20M8P%20V2.0%20User%20Manual.pdf) |
| **EBB SB2209 CAN (RP2040)** | Flashing Katapult (CanBoot) & Klipper via DFU mode over USB/CAN. | [(EBB RP2040 Flashing Guide)](https://global.bttwiki.com/EBB%202209%20CAN%20RP2040.html#klipper-configure) |
| **CanBoot / Katapult Setup** | Bootloader setup for updating CAN toolhead nodes over the CAN bus. | [(Katapult GitHub Repo)](https://github.com/Arksine/katapult) |

### Klipper & System Configuration
| Setup Step | Description | Reference Link |
| :--- | :--- | :--- |
| **Klipper Official Docs** | Primary documentation for `printer.cfg` syntax, macros, and commands. | [(Klipper Documentation)](https://www.klipper3d.org/) |
| **Voron Klipper Configs** | Baseline reference printer configurations for Voron 2.4. | [(Voron 2.4 Klipper GitHub)](https://github.com/VoronDesign/Voron-2/tree/Voron2.4/firmware/klipper_configurations) |
| **CAN Bus Configuration** | Setting up `can0` interface on Linux and querying `canbus_uuid`. | [(Klipper CAN Bus Setup)](https://www.klipper3d.org/CANBUS.html) |
| **VoronTAP Calibration** | Probing configuration, probe offsets, and z-endstop setup. | [(TAP Setup Documentation)](https://github.com/VoronDesign/Voron-Tap/blob/main/config/tap_klipper_instructions.md) |

---

## 3. Overview of `printer.cfg`

In [Klipper 3D printer firmware](https://www.klipper3d.org/), `printer.cfg` serves as the primary configuration file. It acts as the central hub where all hardware parameters, microcontrollers, kinematics, thermal controls, macros, and operational settings are defined or linked.

Rather than maintaining a single monolithic configuration file containing thousands of lines of code, standard practice in Voron builds is to split the configuration modularly using `[include ...]` statements. This modular approach improves readability, simplifies troubleshooting, and makes updating individual sub-systems (such as toolhead boards, custom macros, or filter controls) significantly cleaner.

---

## 4. Configuration Modular Breakdown

Below is a detailed breakdown of each included configuration file within this setup, outlining its purpose and typical contents:

* **`[include mainsail.cfg]`**
  * **Purpose:** Provides default macro definitions, state tracking variables, and web UI communication hooks required for the [Mainsail OS / Web Interface](https://docs.mainsail.xyz/).
  * **Key Components:** Base macros (`PAUSE`, `RESUME`, `CANCEL_PRINT`), display and system state definitions, and idle timeout management.
* **`[include KNOMI.cfg]`**
  * **Purpose:** Configures the [BTT KNOMI Wiki](https://global.bttwiki.com/KNOMI.html) touchscreen UI / status display mounted on the Stealthburner toolhead using configuration macros provided in the [BTT KNOMI GitHub Repository](https://github.com/bigtreetech/KNOMI).
  * **Key Components:** Display update hooks tied to print states (heating, homing, leveling, printing, error) and network/Wi-Fi configuration parameters for display syncing.
* **`[include klipper_backup.cfg]`**
  * **Purpose:** Automates backing up the Klipper configuration directory to a remote Git repository (e.g., GitHub or GitLab).
  * **Key Components:** Shell command macro wrappers (`[gcode_shell_command]`) and automated commit triggers on configuration changes or web interface commands.
* **`[include macros.cfg]`**
  * **Purpose:** Stores user-defined G-code macros and custom print routines.
  * **Key Components:** `PRINT_START` (bed mesh, heating sequence, nozzle wipe, clean), `PRINT_END` (retract, park toolhead, cool down, disable steppers), filament loading routines (`LOAD_FILAMENT`, `UNLOAD_FILAMENT`), and maintenance macros.
* **`[include nevermore.cfg]`**
  * **Purpose:** Controls the Nevermore air filtration system (recirculating active carbon filter under the bed).
  * **Key Components:** Dedicated macros (`NEVERMORE_ON`, `NEVERMORE_OFF`, `NEVERMORE_SLOW`, `NEVERMORE_FAST`) for active carbon filter control.
* **`[include lights.cfg]`**
  * **Purpose:** Manages the addressable Neopixel LEDs in the Stealthburner toolhead (logo, case, and nozzle lights).
  * **Key Components:** `[neopixel stealthburner]` pin assignments and LED count settings, visual status presets (red for heating, white for lighting, flashing blue for homing), and `[Case LED Control]`.
* **`[include canbus-RP2040.cfg]`**
  * **Purpose:** Configures the toolhead expansion board operating via CAN bus with an RP2040 microcontroller (e.g., BTT EBB SB2209 RP2040).
  * **Key Components:** CAN bus `canbus_uuid` interface setting, extruder stepper driver setup, accelerometer (`ADXL345`) setup for Input Shaping, and hotend heater/thermistor definitions.
* **`[include TMC2209.cfg]`**
  * **Purpose:** Defines UART communication parameters and stepper driver configurations for TMC2209 drivers.
  * **Key Components:** `[tmc2209 stepper_x]`, `stepper_y`, `stepper_z`, `extruder` blocks, `run_current` and `hold_current` configuration, `stealthchop_threshold`, and sensorless homing thresholds (`driver_SGTHRS`).
* **`[include fans.cfg]`**
  * **Purpose:** Outlines all active cooling fans across the printer chassis, controller enclosure, and underbed fan management.
  * **Key Components:** Underbed fans (`[fan_generic BedFans]` with auto heatsoak monitoring macros), electronics enclosure cooling fans (`[controller_fan Electronics_Fan]`), and Nevermore filter fan definition (`[fan_generic Nevermore]`).
* **`[include motors.cfg]`**
  * **Purpose:** Defines kinematic motion limits, motor drive steps, rotation distances, and axis bounds.
  * **Key Components:** Axis boundaries (`position_min`, `position_max`, `position_endstop`), microstepping and `rotation_distance` calculations for Quad Gantry Leveling setup, and endstop pin assignments.
* **`[include tap.cfg]`**
  * **Purpose:** Configures the [Voron TAP](https://github.com/VoronDesign/Voron-Tap) nozzle-based Z-probe system.
  * **Key Components:** `[probe]` section definitions (pin assignments, `x_offset: 0`, `y_offset: 0`, probe speed, trigger samples), safety macros (ensuring nozzle stays under 150°C during probing), and `[safe_z_home]` overrides.

---

## 5. Klipper Documentation & Resources

* [Klipper Official Overview & Docs](https://www.klipper3d.org/Overview.html)
* [Klipper Configuration Reference](https://www.klipper3d.org/Config_Reference.html)
* [Klipper G-Code & Macro Reference](https://www.klipper3d.org/G-Codes.html)
* [Klipper Kinematics Documentation](https://www.klipper3d.org/Kinematics.html)
* [Klipper TMC Driver Guide](https://www.klipper3d.org/TMC_Drivers.html)
* [Klipper CAN Bus Setup](https://www.klipper3d.org/CANBUS.html)
* [Voron Design Documentation](https://docs.vorondesign.com/)
* [Mainsail Documentation](https://docs.mainsail.xyz/)