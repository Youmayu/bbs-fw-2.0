# BBSHD/BBS02/TSDZ2 Open Source Firmware

This repository contains open source replacement firmware for Bafang BBSHD and BBS02 motor controllers. It also supports TongSheng TSDZ2 controllers with Bafang-compatible displays when a suitable custom cable is used.

This fork is based on the upstream [danielnilsson9/bbs-fw](https://github.com/danielnilsson9/bbs-fw) project and includes local documentation updates plus compatibility work for displays such as the 860C.

Do not flash or configure the controller while the e-bike battery is charging.

## Documentation

- [Local wiki draft](wiki/Home.md)
- [GitHub wiki](https://github.com/Youmayu/bbs-fw-2.0/wiki)
- [Build from Source](wiki/Build-from-Source.md)
- [Flash Firmware](wiki/Flash-Firmware.md)
- [Configuration Tool](wiki/Configuration-Tool.md)
- [Display Behavior](wiki/Display-Behavior.md)
- [Troubleshooting](wiki/Troubleshooting.md)

The local `wiki/` folder contains the same draft pages prepared for publishing to the GitHub wiki repository.

## Highlights

- Higher current limits without hardware modification, up to the firmware and controller limit.
- Configurable low-voltage cutoff for different battery voltages.
- Smooth throttle and pedal-assist override behavior.
- Two operation mode pages, normally used as standard and sport assist profiles.
- Per-assist-level road speed limits.
- Per-assist-level cadence limits.
- Cruise assist levels, where motor power can stay active without pedal or throttle input after engagement.
- Gradual thermal current limiting.
- Gradual low-voltage current limiting.
- Battery voltage calibration for more accurate low-voltage cutoff and low-voltage rampdown.
- Temperature display on compatible Bafang display fields.
- Optional speed sensor use.
- Optional shift sensor support on BBSHD and BBS02.
- Optional drivetrain pretension.
- Configurable external lights behavior.

## Display Compatibility Notes

Only displays that use the Bafang display protocol can work. Some displays implement that protocol differently, so behavior can still vary by display model.

This fork ignores display work-mode write commands after validating the packet checksum. This prevents displays such as the 860C from continuously forcing ECO or Sport mode and lets the controller-side operation-mode toggle control the selected mode.

The firmware also repurposes some standard Bafang display fields:

- The range field shows temperature by default on BBSHD and BBS02.
- The calories field shows battery voltage.
- Walk mode can show temperature, requested power, or battery percentage in the speed field.
- Display speed-limit writes are ignored; configure speed limits in the BBS-FW configuration tool.

See [Display Behavior](wiki/Display-Behavior.md) for details.

## Supported Hardware

### BBSHD

| Revision | MCU          | Released | Comment |
| -------- | ------------ | -------- | ------- |
| V1.4     | STC15W4K56S4 | ~2017    | V1.3 printed on PCB, sticker with 1.4. |
| V1.5     | IAP15W4K61S4 | ~2019    | V1.4 printed on PCB, sticker with 1.5. |

Do not flash other BBSHD controller revisions unless you accept the risk of bricking the controller.

### BBS02B

There are reported compatibility issues with some BBS02 controllers. Newer BBS02B controllers are more likely to work. Older controllers may not be safe to flash.

| Revision | MCU          | Released | Comment |
| -------- | ------------ | -------- | ------- |
| V1.?     | STC15F2K60S2 | Unknown  | Supported from BBS-FW version 1.1. |
| V1.?     | IAP15F2K61S2 | Unknown  | Supported from BBS-FW version 1.1. |

BBS02A is not tested and is not recommended unless the controller is already bricked and you accept the risk.

### TSDZ2

TSDZ2A/B controllers using the STM microcontroller are supported. This is most TSDZ2 controllers. A custom cable is required to interface with Bafang-compatible displays.

## Build Quick Start

Install `make` and SDCC 4.2, then build from `src/firmware`.

Build BBSHD:

```powershell
make -B all TARGET_CONTROLLER=BBSHD
```

Build BBS02:

```powershell
make -B all TARGET_CONTROLLER=BBS02
```

The generated firmware file is:

```text
src/firmware/bbs-fw.hex
```

This file is overwritten by each build. Save target-specific copies if you build for more than one controller:

```powershell
make -B all TARGET_CONTROLLER=BBSHD
copy bbs-fw.hex bbs-fw-BBSHD.hex

make -B all TARGET_CONTROLLER=BBS02
copy bbs-fw.hex bbs-fw-BBS02.hex
```

## Configuration Tool

The standard Bafang configuration tool is not compatible with this firmware's configuration layout. Use the BBS-FW configuration tool in `src/tool`.

To build it, open:

```text
src/tool/bbs-fw-tool.sln
```

in Visual Studio 2022 with the .NET desktop development workload installed.

## Legal

- Installing this firmware will void your warranty.
- Use this firmware at your own risk.
- Incorrect flashing or incompatible hardware can brick the controller.
- Incorrect configuration can make the bike behave unexpectedly. Test changes carefully in a safe area.
