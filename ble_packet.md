---
layout: page
title: iBeacon packet
permalink: /ibeacon_packet/
---

### Traditional Bluetooth 4 advertisement packet

[Bluetooth Core Specification](https://www.bluetooth.org/en-us/specification/adopted-specifications)
[BlackHat-Ryan](https://media.blackhat.com/us-13/us-13-Ryan-Bluetooth-Smart-The-Good-The-Bad-The-Ugly-and-The-Fix.pdf)

Example adv. packet:

  * xx                # Preamble (1 octet)
  * d6 be 89 8e       # address - allways the same for advertisement packets (6.B.2.1.2 Bluetooth Spec.)
  * PDU
    * 00 11             # ADV_IND adv PDU type - type of adv. packet (6.B.2.3 Bluetooth Spec.)
    * 99 92 b1 eb d7 90 # AdvA (6.B.2.3)
    * 02 01 05 07 02 03 18 02 18 04 18 # AdvData - In this example its capabilities and three UUIDs the device provides.
  * a8 5d ef          # crc data 3 octets

### iBeacon packet disection

iBeacon packet is a standard Bluetooth 4 advertisement packet with a specific format of manufacturers data field

creating iBeacon packet in Linux using tools from [BlueZ Bluetooth protocol stack](http://www.bluez.org/)

{% highlight bash %}
sudo hciconfig hci0 up
sudo hciconfig hci0 noscan
sudo hcitool -i hci0 cmd 0x08 0x0008 1e 02 01 1a 1a ff 4c 00 02 15 e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0 00 00 00 00 c5
sudo hcitool -i hci0 cmd 0x08 0x0006 A0 00 A0 00 03 00 00 00 00 00 00 00 00 07 00
sudo hcitool -i hci0 cmd 0x08 0x000a 01 
{% endhighlight %}

  * 0x08 0x0008 - command: HCI_LE_Set_Advertising_Data (hcitool ogf)
  * 1e (dec: 30) # size (30 octets) (hcitool ocf)
  * 02 # Number of bytes that follow in first AD structure
  * 01 # Flags AD type
  * 1A # Flags value 0x1A = 000011010
    * bit 0 (OFF) LE Limited Discoverable Mode
    * bit 1 (ON)  LE General Discoverable Mode
    * bit 2 (OFF) BR/EDR Not Supported
    * bit 3 (ON)  Simultaneous LE and BR/EDR to Same Device Capable (controller)
    * bit 4 (ON)  Simultaneous LE and BR/EDR to Same Device Capable (Host)
  * 1A # Number of bytes that follow in second (and last) AD structure (26)
  * FF # Manufacturer specific data AD type
  * 4C 00 # Company identifier code (0x004C == Apple)
  * Start of iBeacon data
    * 02 # Byte 0 of iBeacon advertisement indicator
    * 15 # length of uuid + major + minor + TX power lvl (0x15 = 21 bytes = 16+2+2+1)
    * E2 C5 6D B5 DF FB 48 D2 B0 60 D0 F5 A7 10 96 E0 # iBeacon broadcasting Profile UUID (16 octets/bytes (x8bits) = 128bit)
    * 00 00 # for iBeacon minor number (16bit)
    * 00 00 # for iBeacon major number (16bit)
    * c5 # 2’s complement of measured TX power lvl (RSSI at one meter) (0xC5 = 197, 2’s complement = 256-197 = -59 dBm)

Some information referrenced from [here](http://stackoverflow.com/questions/18906988/what-is-the-ibeacon-bluetooth-profile)
