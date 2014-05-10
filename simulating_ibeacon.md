---
layout: page
title: iBeacon simulation
permalink: /ibeacon_simulation/
---

To simulate and pick-up iBeacon packets your system needs to include Bluetooth 4 compatible adapter - this is still very rare for laptops or desktop PCs, therefore we are using Bluetooth 4 USB dongles. 

### Compatible hardware

* tested (Ubuntu 14.04; Raspbian) and working: [Plugable USB Bluetooth 4.0 Low Energy Micro Adapter](http://www.amazon.co.uk/Plugable-Bluetooth-Adapter-Windows-Compatible/dp/B009ZIILLI/ref=sr_1_1?m=A16DOJNYBMLIQ3&s=merchant-items&ie=UTF8&qid=1393582835&sr=1-1)
* tested (Ubuntu 14.04; Raspbian) and working: [Belkin F8T065BF]

### Tools installation

* BlueZ

## Simulation iBeacon packet in Linux

{% highlight bash %}
sudo hciconfig hci0 up
sudo hciconfig hci0 noscan
sudo hcitool -i hci0 cmd 0x08 0x0008 1e 02 01 1a 1a ff 4c 00 02 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 00 00 00 00 c5
sudo hcitool -i hci0 cmd 0x08 0x0006 A0 00 A0 00 03 00 00 00 00 00 00 00 00 07 00
sudo hcitool -i hci0 cmd 0x08 0x000a 01 
{% endhighlight %}

The second hcitool command (0x08 0x0006) is "LE Set Advertising Parameters. The first two bytes A0 00 are the "min interval". The second two bytes A0 00 are the "max interval". In this example, it sets the time between advertisements to 100ms. The granularity of this setting is 0.625ms, so setting the interval to 01 00 sets the advertisement to go every 0.625ms. Setting it to A0 00 sets the advertisement to go every 0xA0*0.625ms = 100ms. Setting it to 40 06 sets the advertisement to go every 0x0640*0.625ms = 1000ms. The fifth byte, 03, sets the advertising mode to non-connectable. With a non-connectable advertisement, the fastest you can advertise is 100ms, with a connectable advertisment (0x00) you can advertise much faster.

The third hcitool command (0x08 0x000a) is "LE Set Advertise Enable". It is necessary to issue this command with hcitool instead of hciconfig, because "hciconfig hci0 leadv 3" will automatically set the advertising rate to the slower default of 1280ms.

  * E2 C5 6D B5 DF FB 48 D2 B0 60 D0 F5 A7 10 96 E0 - iBeacon broadcasting Profile UUID (16 octets)
  * 00 00 - for iBeacon minor number (0)
  * 00 00 - for iBeacon major number (0)
  * c5 - 2’s complement of measured TX power

## Picking up iBeacons in Linux

{% highlight bash %}
hcitool lescan --duplicates
# in other terminal:
hcidump -x -R -Y
{% endhighlight %}
