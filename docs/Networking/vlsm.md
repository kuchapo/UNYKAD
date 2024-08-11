# VLSM

Beispiel:  
Es werden mit Hilfe von VLSM jeweils 800, 400 und 117 Clients in
voneinander isolierten Netze eingeteilt. Wichtig zu beachten ist, dass die
Netz-ID und Broadcast-IP bei der Hostanzahl berücksichtigt werden sollten und
dass man immer mit dem größten Subnetz anfängt und dann immer jeweils das kleinere
nimmt.

Tabelle als Hilfestellung benutzen:

| Hostanzahl    | 1024 | 512  | 256  | 128  | 64   | 32   | 16   | 8    | 4    | 2    |
|---------------|------|------|------|------|------|------|------|------|------|------|
| Subnetze      | 2    | 4    | 8    | 16   | 32   | 64   | 128  | 256  | 512  | 1024 |
| binär         | 512  | 256  | 128  | 64   | 32   | 16   | 8    | 4    | 2    | 1    |

!!! info
    Wenn man nach Hostanzahl geht, dann wird der Strich links von der Zahl gesetzt.  
    'Subnetze' und 'binär' werden hier nur zum besseren Verständnis aufgelistet.

Das Netz 10.4.0.0 wird folgendermaßen segmentiert:

für 800 Hosts:  
10.4.000000|00.00000000 Netz-ID 10.4.0.0/22  
10.4.000000|11.11111111 Broadcast-IP 10.4.3.255/22

für 400 Hosts:  
10.4.0000010|0.00000000 Netz-ID 10.4.4.0/23  
10.4.0000010|1.11111111 Broadcast-IP 10.4.6.127/25

für 117 Hosts:  
10.4.00000110.0|0000000 Netz-ID 10.4.6.0/25  
10.4.00000110.0|1111111 Broadcast-IP 10.4.6.127/25
