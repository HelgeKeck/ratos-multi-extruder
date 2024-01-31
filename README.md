# ROME
RatOS Multi Extruder

A multi extruder feeder to direct extruder solution for RatOS

## Table of Content
- [Intro](#intro)
- [Installation](#installation)
    - [Raspberry](#raspberry)
    - [Moonraker](#moonraker)
    - [RatOS](#ratos)
- [Slicer](#slicer)
    - [G-code](#G-code)
    - [Native](#native)

## Printed Parts
  - [Filament Sensor](https://github.com/HelgeKeck/rome/tree/main/cad/stl/filament_sensors)
  - [MMU Splitter](https://github.com/HelgeKeck/rome/tree/main/cad/stl/mmu_splitter)
  - [Orbiter Mounts](https://github.com/HelgeKeck/rome/tree/main/cad/stl/orbiter_mounts)

# Intro

ROME is MUCH faster then any regular MMU, ERCF or similar setups. The whole filament unloading and loading process is multiple times faster, not only because ROME just has to park the filament behind the y-junction. In its native mode, ROME handles the loading and unloading process and skips the slicer controlled part of it. This process is highly optimized for a specific Hotend / Filament combination. No more configuration of cooling moves, skinnydip, ramming, .... 

You can even set the acceleration for the wipe tower, in combination with the slicer **max speed for the wipe tower** feature, you can speed up the process even more.

With its **Ooze Ex** feature, it lets you use the most oozing hotends on the markets, it still produces a clean wipe tower

Every MM loading and unloading setting in the slicer will be deactivated and replaced with a simple unload macro. 

<img src="https://github.com/HelgeKeck/rome/blob/main/img/1.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/rome_top.jpg" alt="" width="800"/>

Wipe tower, printed with 300mm/s @ 25000 mm²/s acceleration on a Rapido UHF with Prusament PETG. 

<img src="https://github.com/HelgeKeck/rome/blob/main/img/wipe_tower_1.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/wipe_tower_2.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/wipe_tower_3.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/wipe_tower_4.jpg" alt="" width="800"/>

Examples 

<img src="https://github.com/HelgeKeck/rome/blob/main/img/sample.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/sample2.jpg" alt="" width="800"/>
<img src="https://github.com/HelgeKeck/rome/blob/main/img/sample3.jpg" alt="" width="800"/>

# Installation

## Raspberry
```
cd ~/
git clone https://github.com/HelgeKeck/rome.git
bash ~/rome/install.sh
```

## RatOS

In the user overrides section in RatOS, include the base configuration file. 

```ini
[include rome.cfg]      
```

Please make all changes and overrides in this file, there is no need to do that in your printer.cfg

## Moonraker
```ini
[update_manager rome]
type: git_repo
primary_branch: main
path: ~/rome
origin: https://github.com/HelgeKeck/rome.git
is_system_service: False
managed_services: klipper
install_script: install.sh
```

# Slicer 

## G-code 

**Printer Start G-code**
```
ROME_START_PRINT EXTRUDER_TEMP=[first_layer_temperature] BED_TEMP=[first_layer_bed_temperature] CHAMBER_TEMP=[chamber_temperature] TOOL=[initial_tool] WIPE_TOWER={wipe_tower} WIPE_TOWER_X={wipe_tower_x} WIPE_TOWER_Y={wipe_tower_y} WIPE_TOWER_WIDTH={wipe_tower_width} WIPE_TOWER_ROTATION_ANGLE={wipe_tower_rotation_angle} COOLING_TUBE_RETRACTION={cooling_tube_retraction} COOLING_TUBE_LENGTH={cooling_tube_length} PARKING_POS_RETRACTION={parking_pos_retraction} EXTRA_LOADING_MOVE={extra_loading_move} X0={first_layer_print_min[0]} Y0={first_layer_print_min[1]} X1={first_layer_print_max[0]} Y1={first_layer_print_max[1]}
```

**Printer End G-code**
```
ROME_END_PRINT
```

**Printer Tool change G-code**
```
CHANGE_TOOL TOOL=[next_extruder]
```

**Filament Start G-code**

This is just an example, it shows how to configure pressure advance for the used extruders

```
SET_PRESSURE_ADVANCE ADVANCE=0.065 SMOOTH_TIME=0.04
SET_PRESSURE_ADVANCE ADVANCE=0.065 SMOOTH_TIME=0.04 EXTRUDER=rome_extruder_1
SET_PRESSURE_ADVANCE ADVANCE=0.065 SMOOTH_TIME=0.04 EXTRUDER=rome_extruder_2
```

## ROME Modes 

ROME can operate in two different modes, Native and Classic.

The Classic Mode works exactly like the MMU or ERCF. You are responsible to configure the Slicer like you would for the MMU or ERCF.

The Native Mode handles the filament loading and unloading on the Wipe tower. Faster filament changes, less Slicer configuration needed and more control over the process.

## Native 

**Use relative E distances**

Printer Settings->General

<img src="https://github.com/HelgeKeck/rome/blob/main/img/advanced.jpg" alt="" width="320"/>


**Activate additional extruders**

Printer Settings->General

<img src="https://github.com/HelgeKeck/rome/blob/main/img/capabilities.jpg" alt="" width="387"/>


**Dectivate all multi material parameters**

Printer Settings->Single extruder MM setup

<img src="https://github.com/HelgeKeck/rome/blob/main/img/mmu_parameters.jpg" alt="" width="407"/>


**Dectivate advanced wiping volume**

Printer Settings->Single extruder MM setup

<img src="https://github.com/HelgeKeck/rome/blob/main/img/wiping_volume.jpg" alt="" width="462"/>


**Dectivate toolchange temperature**

Filament Settings->Multimaterial

<img src="https://github.com/HelgeKeck/rome/blob/main/img/toolchange_temperature.jpg" alt="" width="400"/>


**Dectivate skinnydip string reduction**

Filament Settings->Multimaterial

<img src="https://github.com/HelgeKeck/rome/blob/main/img/dip.jpg" alt="" width="454"/>


**Dectivate cooling moves and ramming**

Filament Settings->Multimaterial

<img src="https://github.com/HelgeKeck/rome/blob/main/img/toolchange_parameters.jpg" alt="" width="400"/>

<img src="https://github.com/HelgeKeck/rome/blob/main/img/ramming.jpg" alt="" width="764"/>


**Enable wipe tower**

Print Settings->multiple extruders

<img src="https://github.com/HelgeKeck/rome/blob/main/img/wipe_tower.jpg" alt="" width="584"/>
