---
layout: page
title: RF considerations
permalink: /rf/
---

BLE uses 40 channels in 2.45GHz ISM band

Bluetooth LE defines three channels to be used in 2.4GHz spectrum as advertisement channels. These are:

* 2402 MHz (chan. 37), 2426MHz (chan 38), 2480 MHz (chan 39)

There are another 37 channels to be used for BLE traffic - 0-36 with 2MHz channel width:

* ch 37 2402MHz(adv)
* ch 0-10 2404-2424MHz
* ch 38 2426MHz (adv)
* ch 11-36 2428-2478MHz
* ch 39 2480MHz (adv)

### TX power

iPhone 5S uses BCM43342 Bluetooth radio chip - what is its sensitivity (so we can calculate theoretical distance for beacon distance)? But the chip is claimed to be Class 1 TX: Class 1 radios – used primarily in industrial use cases – have a range of ~100 meters. [Broadcom](http://www.broadcom.com/products/Wireless-LAN/802.11-Wireless-LAN-Solutions/BCM4334)

* 0dBm = 1mW
* 4dBm = 2.5mW
