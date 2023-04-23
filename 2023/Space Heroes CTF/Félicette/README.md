# Félicette

Writeup by: [j4asper](https://github.com/j4asper)

---

## Challenge Description

a cat in space, eating a croissant, while starting a revolution.

[chall.jpg.pcap](./files/chall.jpg.pcap)

## Challenge Solution

Python script:

```py
from scapy.all import rdpcap

packets = rdpcap("chall.jpg.pcap")

hex_data = []
for packet in packets:
    hex_data.append(packet.lastlayer().fields['load'])

with open("Felicette.jpg", "wb") as f:
    for i in hex_data:
        f.write(i)
```
