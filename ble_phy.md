---
layout: page
title: BLE PHY Layer
permalink: /ble_phy/
---

  * GFSK +/- 250kHz, 1MBit/sec
  * hopping
  * 40 chans in 2.45GHz

### Hopping

  * hop along 37 chans
  * one packet per chan
  * Next channel = (channel + hop increment) mod 37 (3 → 10 → 17 → 24 → 31 → 1 → 8 → 15 → ... hop increment = 7)
