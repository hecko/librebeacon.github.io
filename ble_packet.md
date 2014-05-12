---
layout: page
title: iBeacon packet
permalink: /ibeacon_packet/
---

### Traditional Bluetooth 4 advertisement packet

[Bluetooth Core Specification](https://www.bluetooth.org/en-us/specification/adopted-specifications)
[BlackHat-Ryan](https://media.blackhat.com/us-13/us-13-Ryan-Bluetooth-Smart-The-Good-The-Bad-The-Ugly-and-The-Fix.pdf)

Example adv. packet (6.B.2.1 [Bluetooth Core Specification]) (data packets are very similar, but this description only applies to advetisement packets):

  * aa                # Preamble (1 octet) - this should be 10101010b for advertisement packets (fixed)
  * d6 be 89 8e       # access address - allways the same for advertisement packets (0x8E89BED6) (fixed)
  * PDU (2-39 octects) - (6.B.2.3 of [Bluetooth Core Specification])
    * Header (16 bits - 2 bytes)
      * 4 bits - PDU type (b3b2b1b0)
        * 0010 - ADV_NONCONN_IND <- used for iBeacon (non-connectable undirected advertising event)
      * 2 bits - RFU ( 00 used for iBeacon)
      * 1 bit  - TxAddr <- 0 when AdvA is public, 1 when random (in Payload) (random (1) used for iBeacon) (fixed)
      * 1 bit  - RxAddr (0 used for iBeacon) (fixed)
      * 6 bits - Length - payload length field, valid vaue: 6-37 (36 used for iBeacon)
      * 2 bits - RFU (00 used for iBeacon)
    * Payload (as big as the length field in the header indicates) (6-37 octects) (6.B.2.3.1.2)
      * AdvA (6 octets) - Advertisers public or random device address (as indicated by TxAddr) -> random for iBeacon
      * AdvData (0-31 octets)
  * crc data 3 octets
  * First preamble is transmitted, then PDU, then CRC
  * Shortest packet is 80 bits, longest is 376 bits long.

  * AdvData for iBeacon
    * 02 # Number of bytes that follow in first AD structure
    * 01 # Flags AD type
    * 04 (5 LSB => following flags)
      * bit 0 (OFF)  LE Limited Discoverable Mode
      * bit 1 (OFF)  LE General Discoverable Mode
      * bit 2 (ON)   BR/EDR Not Supported
      * bit 3 (OFF)  Simultaneous LE and BR/EDR to Same Device Capable (controller)
      * bit 4 (OFF)  Simultaneous LE and BR/EDR to Same Device Capable (Host)
    * 1A - Number of bytes (in AdvData -> 1A -> 26 bytes) (fixed)
    * FF - Manufacturer specific data follows (fixed)
      * Company identifier (4c 00 = Apple), according to the Assigned Numbers from Bluetooth SIG (fixed)
      * everything from this point is completely iBeacon-specific
          * 02 # iBeacon indicator (fixed)
          * Lengts of AdvData (uuid + minor + major + tx power -> 0x15 = 21 bytes)
          * iBeacon UUID (16 oct.)
          * iBeacon minor number (16bit)
          * iBeacon major number (16bit)
          * c5 # 2’s complement of measured TX power lvl (RSSI at one meter) (0xC5 = 197, 2’s complement = 256-197 = -59 dBm)

creating iBeacon packet in Linux using tools from [BlueZ Bluetooth protocol stack](http://www.bluez.org/)

UUID:      t3st2343 (74337374-3233-3433-0000-000000000000)
Major no.: 0        (0000)
Minor no.: 0        (0000)

{% highlight bash %}
sudo hciconfig hci0 up
sudo hciconfig hci0 noscan
sudo hcitool -i hci0 cmd 0x08 0x0008 1e 02 01 04 1a ff 4c 00 02 15 74 33 73 74 32 33 34 33 00 00 00 00 00 00 00 00 00 00 00 00 c5
sudo hcitool -i hci0 cmd 0x08 0x0006 A0 00 A0 00 03 00 00 00 00 00 00 00 00 07 00
sudo hcitool -i hci0 cmd 0x08 0x000a 01
{% endhighlight %}

Some information referrenced from [here](http://stackoverflow.com/questions/18906988/what-is-the-ibeacon-bluetooth-profile)
