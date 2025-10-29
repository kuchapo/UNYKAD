# FLSM

- FLSM = Fixed Length Subnet Mask
- Alle Subnetze gleich groß (gleiche Prefixlänge)


## Beispiel

4 gleich große Netze aus 192.168.0.0/24 werden benötigt:

- aus 2 zusätzlichen Bits kann man 4 Netze bilden, da 2^2=4 (→ /26)
- /26 = 255.255.255.192 = 64 Adressen (62 Hosts nutzbar)
- 4 Subnetze (Schrittweite 64 im letzten Oktett):
    1. 192.168.0.0/26 Hosts 1–62 BC 63
    2. 192.168.0.64/26 Hosts 65–126 BC 127
    3. 192.168.0.128/26 Hosts 129–190 BC 191
    4. 192.168.0.192/26 Hosts 193–254 BC 255

Warum geht das?

- /24 hat 256 Adressen
- /26 hat 64 Adressen
- 256 / 64 = 4 Subnetze

Nur bestimmte Anzahlen möglich:

- zusätzliche Netzbits → Anzahl Subnetze = 2^n
    - 1 Bit → 2 Netze
    - 2 Bit → 4 Netze
    - 3 Bit → 8 Netze
    - 4 Bit → 16 Netze
    - 5 Bit → 32 Netze
    - 6 Bit → 64 Netze

→ Aus 3 Bits kann man nicht exakt 7 gleich große Netze bauen, deswegen baut man 8 und benutzt 7.

## Binäre Veranschaulichung

Maske /26 = 255.255.255.192 =  
11111111.11111111.11111111.11000000  
(erste 26 Bits = Netz, Rest = Host)

- 192.168.0.0 =  
  11000000.10101000.00000000.00000000
- 192.168.0.64 =  
  11000000.10101000.00000000.01000000
- 192.168.0.128 =  
  11000000.10101000.00000000.10000000
- 192.168.0.192 =  
  11000000.10101000.00000000.11000000
