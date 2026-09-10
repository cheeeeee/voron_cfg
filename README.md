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
- [5. Supporting & System Configurations](#5-supporting--system-configurations)
- [6. Klipper Documentation & Resources](#6-klipper-documentation--resources)

---

## 1. Hardware List & Bill of Materials

### Core Control & Power Electronics
| Component | Qty | Notes & Reference Links |
| :--- | :---: | :--- |
| **BTT Manta M8P + CB1** | 1 | Main control board (v2.0) and Compute Module host. [(Manta M8P Repo)](https://github.com/bigtreetech/Manta-M8P) |
| **BTT EBB SB2209 CAN (RP2040)** | 1 | Toolhead expansion board. [(BTT Wiki)](https://global.bttwiki.com/EBB%202209%20CAN%20RP2040.html) \| [(BTT EBB Repo)](https://github.com/bigtreetech/EBB) |
| **TMC2209 Stepper Drivers** | 6 (+1) | 6x on Manta M8P (X, Y, Z0, Z1, Z2, Z3) + 1x integrated on EBB SB2209 (Extruder). [(BTT Wiki)](https://global.bttwiki.com/TMC2209.html) \| [(BTT Driver Repo)](https://github.com/bigtreetech/BIGTREETECH-Stepper-Motor-Driver) |
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
| **PT1000 Temperature Sensor** | 1 | 2-wire RTD probe (1000Ω at 0°C); sampled via MAX31865 RTD amplifier over SPI on EBB SB2209. |
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
| **Filament Runout Sensor** | 1 | Switch sensor connected to `EBBCan:gpio21` (triggers `M600` pause/park macro). |
| **Cooling & Filtration Fans** | 7 | 1x 5015 part fan, 1x 4010 hotend fan, 3x underbed fans (`BedFans`), 1x Nevermore filter fan, 1x electronics bay fan. |
| **Stealthburner & Case LEDs** | -- | Addressable Neopixel toolhead harness (`sb_leds`, GRBW) & 24V chamber lighting strip (`caselight`). |

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

Key machine dynamics and features configured directly in `printer.cfg` include:
* **Core Kinematics & Velocity Limits:** CoreXY motion with `max_velocity: 500`, `max_accel: 5000` (tuned for the `3hump_ei` input shaper profile), `max_z_velocity: 30`, `max_z_accel: 350`, `minimum_cruise_ratio: 0.5`, and `square_corner_velocity: 5.0`.
* **Microcontroller Interfaces:** Dual-MCU architecture using CAN bus UUIDs — the baseboard BTT Manta M8P v2.0 (`canbus_uuid: 79a09a212663`) and the toolhead BTT EBB SB2209 CAN RP2040 (`canbus_uuid: 4cc248f5b9f1`).
* **Thermal Monitoring:** Dedicated MCU sensors monitoring temperature on the Manta M8P (`[temperature_sensor MCU]`) and CB1 host (`[temperature_sensor CB1]`).
* **Advanced Motion & Slicing Features:** `[exclude_object]` (supports cancel-object and Moonraker/Klipper adaptive bed meshing), `[gcode_arcs]` (resolution 0.3mm for smooth ArcWelder / G2-G3 moves), `[force_move]` (`enable_force_move: True`), and `[skew_correction]`.
* **State Persistence:** `[save_variables]` writing persistent variables to `~/printer_data/config/variables.cfg`.

Rather than maintaining a single monolithic configuration file containing thousands of lines of code, standard practice in Voron builds is to split the configuration modularly using `[include ...]` statements. This modular approach improves readability, simplifies troubleshooting, and makes updating individual sub-systems (such as toolhead boards, custom macros, or filter controls) significantly cleaner.

---

## 4. Configuration Modular Breakdown

Below is a detailed breakdown of each included configuration file within this setup, outlining its purpose and current parameters:

* **`[include mainsail.cfg]`**
  * **Purpose:** Provides default macro definitions, state tracking variables, and web UI communication hooks required for the [Mainsail OS / Web Interface](https://docs.mainsail.xyz/).
  * **Key Components:** Base macros (`PAUSE`, `RESUME`, `CANCEL_PRINT`), display and system state definitions, and idle timeout management.
* **`[include KNOMI.cfg]`**
  * **Purpose:** Configures the [BTT KNOMI](https://global.bttwiki.com/KNOMI.html) round touchscreen UI mounted on the Stealthburner toolhead using hooks from the [BTT KNOMI GitHub Repository](https://github.com/bigtreetech/KNOMI).
  * **Key Components:** Global status tracking (`[gcode_macro _KNOMI_STATUS]`), wrapper hooks for `M109` (nozzle heating), `G28` (homing), and `QUAD_GANTRY_LEVEL` (QGL), plus bed-heating integration hooks called from `fans.cfg`.
* **`[include klipper_backup.cfg]`**
  * **Purpose:** Automates backing up the Klipper configuration directory to a remote Git repository (e.g., GitHub or GitLab).
  * **Key Components:** Shell command macro wrappers (`[gcode_shell_command]`) and the `update_git` macro (supports optional commit message via `MESSAGE="commit note"`).
* **`[include macros.cfg]`**
  * **Purpose:** Stores user-defined print routines, filament handling, preheat workflows, and maintenance macros.
  * **Key Components:**
    * `PRINT_START`: Dynamic print initiation macro. Reads bed, extruder, and chamber temperatures from the slicer. If the bed is >90°C, turns on part cooling and bed/Nevermore circulation fans (`BEDFANSFAST`, `NEVERMORE_FAST`) and checks bed temperature — if cold, waits for heatsoak/chamber stabilization, or skips soaking if the bed is already hot. Preheats nozzle to 150°C for safe TAP probing, executes `quad_gantry_level`, homes Z, runs adaptive bed mesh (`BED_MESH_CALIBRATE ADAPTIVE=1`), heats nozzle to print temp, and runs an extrusion purge line.
    * `PRINT_END`: Safe finishing sequence. Retracts 5mm filament, turns off heaters, clears bed mesh, parks nozzle at the rear center (`y_max - 2`), turns off fans, and disables steppers.
    * `M600`: Manual filament change pause macro. Safely lifts Z, retracts filament to clear the hotend melt zone, and parks at front center for easy access.
    * `LOAD_FILAMENT` & `UNLOAD_FILAMENT`: Automated filament management macros with intelligent temperature verification (auto-heats to 230°C default if below 200°C), melt-zone push/tip-shaping, and high-speed retracts.
    * `PREHEAT` & `PREHEAT_OFF`: Dedicated chamber and bed warmup routines (default bed 105°C, nozzle 150°C). Automatically homes if needed, parks the toolhead in bed center at Z50 to warm the gantry and TAP sensor, enables the Nevermore filter and 50% part cooling circulation when bed >= 90°C, and supports an optional countdown timer (e.g. `PREHEAT SOAK=15`).
    * `PARK` & `G32`: Quick toolhead center park macro and complete G28 + QGL + G28 sequence.
* **`[include nevermore.cfg]`**
  * **Purpose:** Provides user control macros for the Nevermore active carbon recirculating air filter element.
  * **Key Components:** Dedicated macros (`NEVERMORE_ON` with optional `SPEED`, `NEVERMORE_OFF`, `NEVERMORE_SLOW` at 50%, `NEVERMORE_FAST` at 100%) targeting the `[fan_generic Nevermore]` fan defined in `fans.cfg`.
* **`[include lights.cfg]`**
  * **Purpose:** Manages the addressable Neopixel LEDs in the Stealthburner toolhead and chamber LED illumination.
  * **Key Components:** `[neopixel sb_leds]` pin assignment (`EBBCan:gpio16`), 3 LEDs (chain index 1: logo, 2-3: nozzle) in `GRBW` order; standard status macros (`STATUS_READY`, `STATUS_OFF`, `STATUS_BUSY`, `STATUS_HEATING`, `STATUS_LEVELING`, `STATUS_HOMING`, `STATUS_CLEANING`, `STATUS_MESHING`, `STATUS_CALIBRATING_Z`, `STATUS_PRINTING`); and chamber LED strip PWM control via `[output_pin caselight]` on pin `PA3` (HE2 header).
* **`[include canbus-rp2040.cfg]`**
  * **Purpose:** Configures the BTT EBB SB2209 CAN (RP2040) toolhead board over CAN bus.
  * **Key Components:**
    * Toolhead MCU (`[mcu EBBCan]` with `canbus_uuid: 4cc248f5b9f1`) and CAN bus power control rail (`[output_pin canbus_power]` on `PA5`).
    * Clockwork2 extruder: Moons NEMA14 motor (`50:10` gear ratio, `rotation_distance: 21.9963`), heater cartridge on `EBBCan:gpio7`, and tuned pressure advance (`pressure_advance: 0.04`).
    * Extruder stepper driver: `[tmc2209 extruder]` on `EBBCan:gpio20` (`run_current: 0.60`, `interpolate: False`).
    * Temperature sensing: PT1000 RTD hotend sensor sampled through onboard `MAX31865` amplifier chip over SPI (`gpio8-11`), plus onboard toolhead thermistor `[temperature_sensor EBB_NTC]` on `EBBCan:gpio28`.
    * Input Shaper & Resonance Testing: Onboard `[adxl345]` accelerometer with tuned shaper profile (`3hump_ei`, X: 90.6 Hz, Y: 71.6 Hz).
    * Fans: 5015 part cooling fan (`EBBCan:gpio14`) and 4010 hotend fan (`EBBCan:gpio13`).
    * Runout Sensor: `[filament_switch_sensor filament_sensor]` on `EBBCan:gpio21` configured with `runout_gcode: M600`.
* **`[include TMC2209.cfg]`**
  * **Purpose:** Defines UART communication parameters, motor currents, and StallGuard sensorless homing for the 6 mainboard stepper drivers.
  * **Key Components:**
    * Drivers configured: `[tmc2209 stepper_x]`, `stepper_y`, `stepper_z`, `stepper_z1`, `stepper_z2`, and `stepper_z3`.
    * Motor currents: `run_current: 1.000A` across X, Y, and all 4 Z steppers.
    * Operating mode: `interpolate: False` and `stealthchop_threshold: 0` (pure SpreadCycle for consistent positioning accuracy).
    * Sensorless homing: StallGuard sensitivity `driver_SGTHRS: 80` on X (`^PF4`) and Y (`^PF3`). (Extruder driver is hosted on EBB in `canbus-rp2040.cfg`).
* **`[include fans.cfg]`**
  * **Purpose:** Controls chassis, electronics bay, underbed circulation, and Nevermore filter fans, as well as AC bed heater control.
  * **Key Components:**
    * Electronics cooling: `[controller_fan Electronics_Fan]` on `PA4` triggered by bed/extruder heaters and stepper activity.
    * Underbed circulation: `[fan_generic BedFans]` spanning 3 fans using `multi_pin:bed_fan_pins` (`PF7, PF9, PF6`) with automated temperature-triggered monitoring loop (`_BEDFANVARS`, `delayed_gcode bedfanloop`, and overrides for `SET_HEATER_TEMPERATURE`, `M140`, `M190`, `TURN_OFF_HEATERS`).
    * Nevermore filter fan: `[fan_generic Nevermore]` on `PF8`.
    * AC Bed Heater: `[heater_bed]` driven via SSR on `PA1` with bed thermistor (Generic 3950) on `PB0` (pullup 2200).
* **`[include motors.cfg]`**
  * **Purpose:** Defines kinematic limits, motor drive steps, rotation distances, and axis travel boundaries.
  * **Key Components:**
    * X axis (Motor 1 / B motor): 32 microsteps, 40mm rotation distance, limits 0 to 355 mm (`position_max: 355`, `position_endstop: 355`), sensorless endstop `tmc2209_stepper_x:virtual_endstop`.
    * Y axis (Motor 2 / A motor): 32 microsteps, 40mm rotation distance, limits 0 to 320 mm (`position_max: 320`, `position_endstop: 320` tuned to prevent physical gantry overtravel), sensorless endstop `tmc2209_stepper_y:virtual_endstop`.
    * Z axes (Motors 3, 5, 6, 7 for Quad Gantry Leveling): 32 microsteps, 40mm rotation distance with `gear_ratio: 80:16`, `position_max: 340`, and `position_min: -15.0` (provides headroom for probe travel during initial unlevel QGL passes). Virtual endstop assigned to `probe:z_virtual_endstop`.
* **`[include tap.cfg]`**
  * **Purpose:** Configures the [Voron TAP](https://github.com/VoronDesign/Voron-Tap) nozzle-based optical Z-probe system, Quad Gantry Leveling routines, and bed mesh limits.
  * **Key Components:**
    * TAP Probe: `[probe]` on `^EBBCan:gpio22`, `speed: 2.5`, `lift_speed: 10.0`, 3 samples median, retract 2.5mm, tolerance 0.02mm, 5 retries.
    * Safety: `activate_gcode` script that automatically verifies nozzle temperature and lowers/waits for <= 150°C before probing to safeguard PEI sheets.
    * Quad Gantry Leveling: `[quad_gantry_level]` with gantry corners `(-60,-10)` and `(410,390)`, 4-point probe pattern `(25,25)`, `(25,300)`, `(325,300)`, `(325,25)`, `speed: 250`, `retry_tolerance: 0.025`, and 10 retries.
    * Bed Mesh: `[bed_mesh]` speed 400 mm/s, boundaries `(15,15)` to `(335,295)`, 5x5 grid, bicubic algorithm.
    * Timeout: `[idle_timeout]` set to 1800s (30 minutes).
* **`[include homing.cfg]`**
  * **Purpose:** Provides sensorless homing macros and customized multi-axis homing override routines.
  * **Key Components:**
    * Sensorless homing macros `_HOME_X` and `_HOME_Y` that dynamically lower stepper current to 0.7A during homing moves to prevent frame/belt strain, back off 10mm, pause 2 seconds (`G4 P2000`) for StallGuard registers to clear, restore standard run currents (1.0A), and ensure absolute coordinates (`G90`) are preserved.
    * `[homing_override]` for XYZ: Performs a 10mm Z hop, automatically homes X and Y before Z homing, moves the nozzle to bed center (X175 Y175), performs TAP Z homing (`G28.1 Z`), and elevates Z to 10mm.

---

## 5. Supporting & System Configurations

Beyond the printer kinematics and macros, the repository tracks supporting host services and environment configurations:

* **`variables.cfg`**
  * Managed by Klipper's `[save_variables]` module (referenced in `printer.cfg` as `~/printer_data/config/variables.cfg`) to persist custom state variables and settings across reboots.
* **`moonraker.conf`**
  * Central API and system management server configuration. Defines trusted client subnets, CORS origins (`*.lan`, `*.local`, Mainsail, Fluidd), and automated `[update_manager]` tracking for:
    * `mainsail` & `mainsail-config`
    * `crowsnest` (camera streaming)
    * `sonar` (wireless keepalive)
    * `timelapse` (Moonraker print timelapse)
    * `print_area_bed_mesh`
    * `KlipperScreen`
    * `klipper-backup` (automated git repository sync)
    * `fluidd`
    * `klipper-macros`
* **`KlipperScreen.conf`**
  * User interface preferences and display sort configurations for the BTT HDMI5 capacitive touchscreen.
* **`sonar.conf`**
  * Configuration for the Sonar network keepalive daemon, periodically sending ICMP pings to ensure reliable Wi-Fi connectivity on the CB1 host.

---

## 6. Klipper Documentation & Resources

* [Klipper Official Overview & Docs](https://www.klipper3d.org/Overview.html)
* [Klipper Configuration Reference](https://www.klipper3d.org/Config_Reference.html)
* [Klipper G-Code & Macro Reference](https://www.klipper3d.org/G-Codes.html)
* [Klipper Kinematics Documentation](https://www.klipper3d.org/Kinematics.html)
* [Klipper TMC Driver Guide](https://www.klipper3d.org/TMC_Drivers.html)
* [Klipper CAN Bus Setup](https://www.klipper3d.org/CANBUS.html)
* [Voron Design Documentation](https://docs.vorondesign.com/)
* [Mainsail Documentation](https://docs.mainsail.xyz/)