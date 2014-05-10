---
layout: page
title: nRF51 based design
permalink: /nrf51/
---

### Introduction

nRF51822 is one of the great little Bluetooth 4-enabled SoC chips. It is made by [Nordic Semiconductor](http://www.nordicsemi.com/) and is widely available.

nRF51822 does not need any special hardware to implement iBeacon advertisement packets, just a specific format of the advertisement packet needs to be constructed by firmware.

### Schematics

To be added soom.

### Firmware

To be added soon.

### Flashing the firmware

To flast nRF51822 you will have to get the Segger programmer which is a part of the nRF51822 Development Kit (nRF51822-DK). Only two pins are needed to program the chip - GND, VCC, SWDIO and SWCLK. SWDIO pin is also called nRESET pin.

Segger programmer in Nordic development kit uses 9-pin 0.05'' Samtec FTSH connector as defined by ARM. This is slightly problematic, but you can use four wires to directly wire from the female connector to the pins on LibreBeacon board.

![programming header](http://www.segger.com/cms/admin/uploads/imageBox/J-Link_9-pin_Cortex-M_Adapter.png)

Board needs to be powered while being programmed using battery or in parallel with power source attached to GND and VCC leads rom the programmer to the board.

### Battery considerations

nRF51822 uses about 55uA when in sleep mode and about 11mA when broadcasting iBeacon packets at 0dBm (1mW) transmit power. CR2032 battery has about 200mAh capacity.

### TX (transmit) power vs power use

Accepted transmit power values for nRF51822 are:
-40, -30, -20, -16, -12, -8, -4, 0, and 4 dBm
-40 is actually -30 as defined by nRF51 documentation

From lowest, to highest. Default is 0dBm (1mW). 4dBm is ~2.5mW of TX power.

Power use at different TX settings:
0dBm -> ~11mA
