# Project RAVEN

Solid-state particle detector for a rocket payload. Detects cosmic radiation
throughout flight and logs each hit with altitude, to build up counting
statistics post-flight.

Built in KiCad. Mixed-signal board: an ESP32-S3 reads two independent
silicon-diode detector channels and logs events. 

## How it works 

A cosmic particle hits a reverse-biased photodiode and dumps a few femtocoulombs
of charge. A charge-sensitive amplifier turns that into a ~7 mV pulse, a
comparator checks it against a tunable threshold, and the ESP32 counts the
resulting digital edge on a hardware interrupt. Two diodes on two independent
channels double the sensitive area without doubling the noise.

## Repository layout

The KiCad project has five schematic sheets:
Using the ICLR Board Template

| Sheet | Contents |
|-------|----------|
| Root | ESP32-S3, status LEDs, connectors, test points |
| Power | Three isolated rails off a ~7.4V battery: +50V bias, +5V analogue, +3.3V digital |
| USB | USB-C receptacle, CC resistors, ESD protection |
| CAN-BUS | SN65HVD230 transceiver, termination, ESD |
| Detector | The signal chain: diodes, charge amps, comparators (2 channels) |

## specs

- **Enclosure:** Circa 90 x 90 x 100 mm (TBD), light-tight detector housing
- **Power:** ~1.5 W from a ~7.4V battery (cell TBC)
- **Detector:** 2x BPW34 photodiodes, biased at +50V
- **Front-end:** ADA4817-1 charge amp, TLV3201 comparator, per channel
- **MCU:** ESP32-S3

## Status

Work in progress. Schematic of the detector front-end is nearly
complete; layout, bring-up, and firmware still to come.

**Outstanding before layout:**
- Confirm comparator edge polarity on the bench (rising vs falling) before
  writing interrupt firmware
- Run ERC (check for missing PWR_FLAG on the +50V / +5V nets)
- SD-card logging and altitude-sensor integration not yet started

## Firmware

Firmware is set up for PlatformIO. Clone the repo and open in PlatformIO to
build and flash.


