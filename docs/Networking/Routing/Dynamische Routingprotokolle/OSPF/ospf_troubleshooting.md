# OSPF Troubleshooting

## Beispiel

![OSPF Troubleshooting](assets/ospf_troubleshooting.drawio.svg)

Router A:

=== "show ip routes"
    | Typ | Zielnetz    | Subnetzmaske | Ausgangsport    | next Hop |
    | --- | ----------- | ------------ | --------------- | -------- |
    | C   | 172.19.0.0  | /24          | FastEthernet0/0 | -        |
    | C   | 10.0.1.0    | /24          | Loopback0       | -        |
    | C   | 192.168.0.8 | /30          | Serial0/0/0     | -        |
    | C   | 192.168.0.0 | /30          | Serial0/1/0     | -        |

=== "show ip interface brief"
    | Interface       | IP-Adress   | OK? | Method | Status                | Protocol |
    | --------------- | ----------- | --- | ------ | --------------------- | -------- |
    | FastEthernet0/0 | 172.19.0.1  | YES | manual | up                    | up       |
    | FastEthernet0/1 | unassigned  | YES | TFTP   | administratively down | down     |
    | Serial0/0/0     | 192.168.0.8 | YES | manual | up                    | up       |
    | Serial0/1/0     | 192.168.0.0 | YES | manual | up                    | up       |
    | Loopback0       | 10.0.1.1    | YES | manual | up                    | up       |

Wir befinden uns auf dem PC links und verbinden uns via Rollover-Kabel mit Router A. Mit dem Befehl `show ip protocols` lassen wir uns die OSPF-Konfiguration anzeigen und sehen, dass der Router aufgrund seiner Loopback-Adresse die Router ID 10.0.1.1 hat.

Wenn man sich nun mit dem Router B (Router ID: 10.0.0.1) verbindet, dann kommt die Fehlermeldung dass es eine duplizierte Router ID auf dem Router mit der IP 192.168.0.5 gibt. Router B und C besitzen also dieselbe Router ID. Um das zu ändern, muss man zuerst die Loopback-Adresse von Router C anpassen (z. B. `ip address 10.0.3.1 255.255.255.0` + `no shut`) und danach den OSPF-Prozess auf dem Router mit dem folgenden Befehl neu starten:

```cli
clear ip ospf process
```

Router C hat nur 10.0.0.1 (Router B) als Neighbor ID. Das liegt daran, dass das Interface S0/0/0 von Router C falsch konfiguriert ist. In `show run int S0/0/0` muss statt `ip address 192.68.0.2 255.255.255.252` richtiger Weise `ip address 192.168.0.2 255.255.255.252` stehen. Die Routing Tabelle von Router C sieht nun folgendermaßen aus:

| Typ | Zielnetz    | Subnetzmaske | Ausgangsport    | next Hop    |
| --- | ----------- | ------------ | --------------- | ----------- |
| O   | 172.19.0.0  | /24          | Serial0/0/0     | 192.168.0.1 |
| C   | 172.21.0.0  | /24          | FastEthernet0/1 | -           |
| C   | 10.0.3.0    | /24          | Loopback0       | -           |
| O   | 10.0.0.1    | /32          | Serial0/1/0     | 192.168.0.6 |
| O   | 192.168.0.8 | /30          | Serial0/0/0     | 192.168.0.1 |
| C   | 192.168.0.0 | /30          | Serial0/0/0     | -           |
| C   | 192.168.0.4 | /30          | Serial0/1/0     | -           |

Router B hat keine Nachbarschaft mit Router A. Nach einem `show ip protocols` sieht man, dass die OSPF-Area falsch eingetragen wurde. Um die falsche Konfiguration zu finden kann man den Befehl `show run | b router ospf <Number>` ausführen. Bei dem falschen Befehl kann man nun, nachdem man `router ospf <Number>`  eingegeben hat, ein `no` davor schreiben und den richtigen Befehl bzw. die richtigen Befehle mit der richtigen Area ausführen.

Es fehlt auf dem Router B weiterhin die Route in das Netz `172.21.0.0`. Aus diesem Grund verbinden wir uns mit dem Rollover-Kabel mit dem Router C. Dort sehen wir mit `show ip protocols`, dass das Netz 172.20.0.0 geworben wird. Das muss später entfernt werden. Das richtige Netz kann nun mit `router ospf <Number>` und `network 172.21.0.0 0.0.0.255 area 0` hinzugefügt werden.

Bei Router A muss nun die Routing-Tabelle folgendermaßen aussehen:

| Typ | Zielnetz    | Subnetzmaske | Ausgangsport    | next Hop     |
| --- | ----------- | ------------ | --------------- | ------------ |
| C   | 172.19.0.0  | /24          | FastEthernet0/0 | -            |
| O   | 172.21.0.0  | /24          | Serial0/1/0     | 192.168.0.2  |
| O   | 172.20.0.0  | /24          | Serial0/0/0     | 192.168.0.10 |
| C   | 10.0.1.0    | /24          | Loopback0       | -            |
| O   | 10.0.0.1    | /32          | Serial0/0/0     | 192.168.0.10 |
| C   | 192.168.0.8 | /30          | Serial0/0/0     | -            |
| C   | 192.168.0.0 | /30          | Serial0/1/0     | -            |
| O   | 192.168.0.4 | /30          | Serial0/0/0     | 192.168.0.10 |
| O   | 192.168.0.4 | /30          | Serial0/1/0     | 192.168.0.2  |

## Loopback-Adressen werben!

Es ist Best Practice, immer die Loopback-Adressen zu announcen, weil man sich nur eine feste IP-Adresse pro Gerät merken muss und diese unabhängig von den physischen Interfaces stabil bleibt.  
Für das Management setzt man zusätzlich oft ein Out-of-Band-Netz (OOB) ein. Damit kann man das Gerät auch dann erreichen, wenn das normale Produktionsnetz für den üblichen Traffic komplett ausgefallen ist.

Aus diesem Grund muss auf Router A, B und C noch die Loopback-Adresse in der Routing Tabelle ergänzt werden, damit diese auch geroutet werden können.

=== "Router A"
    ```cli
    router ospf <Number>
    network 10.0.1.0 0.0.0.255 area 0
    ```

=== "Router B"
    ```cli
    router ospf <Number>
    network 10.0.0.0 0.0.0.255 area 0
    ```

=== "Router C"
    ```cli
    router ospf <Number>
    network 10.0.3.0 0.0.0.255 area 0
    ```

!!! info
	Wichtig: In `show ip route` wird nicht sowas wie 10.0.1.0/24 stehen, sondern 10.0.1.1/32, da es bereits folgendermaßen definiert worden ist:
	
	```cli
	interface Loopback0
	ip address 10.0.1.1 255.255.255.255
	```

    Wenn man keine Subnetzmaske angibt, dann interpretiert der Router die Loopback-Adresse automatisch als /32.

## Load-Balancing Problem

Wenn Router C ebenfalls eine Route zu **172.20.0.0/24** besitzt (wie auch Router B) und vom linken PC ein Ping an **172.20.0.8** geschickt wird, kann es passieren, dass nur ein Teil der ICMP‑Antworten ankommt. Dieses Verhalten nennt man ECMP (Equal-Cost Multi-Path Routing). Um dies zu vermeiden, kann man mit `no network 172.20.0.0 0.0.0.255` verhindern, dass Router C dieses Netz per OSPF advertised. Die direkt angeschlossene Route zu **172.20.0.0/24** bleibt jedoch weiterhin in der lokalen Routing-Tabelle bestehen (besser wäre es jedoch, es trotzdem zu whipen mit `default int <Interface>`).

Beim Einsatz des `network`‑Befehls in OSPF werden **primär nur die Netze von direkt verbundenen Interfaces (inklusive Loopbacks)** in OSPF aufgenommen und an andere Router advertised. Andere Netze gelangen nur über zusätzliche Mechanismen wie **`redistribute`** in die OSPF-Domäne.

## Nur die nötigen Netze werben

Um Probleme zu vermeiden, ist es ratsam, im OSPF nur die wirklich benötigten Netze zu werben. Viele Anfänger verwenden den Befehl:

```cli
network 0.0.0.0 0.0.0.0 area 0
```

Dadurch werden **alle direkt verbundenen Netze** in OSPF aufgenommen. Das wirkt zwar bequem, kann aber künftig zu Problemen führen – etwa durch unnötige Einträge in der Routing-Tabelle oder unerwünschtes **Load-Balancing (ECMP)**.

## Bandbreite modifizieren

Die summierten OSPF-Kosten entlang eines Pfades stellen das oberste Entscheidungsmerkmal bei der Routenwahl dar. Mit dem Befehl `show int <Interface>` kann man unter `BW <Number> Kbit/sec` die aktuelle Bandbreite einsehen. Dort steht 1544 Kbit/sec, was 1,544 Mbit/sec entspricht, was sehr wenig ist. Aus diesem Grund benutzt man normalerweise heutzutage auch kaum seriellen Verbindungen zwischen Routern mehr.

## Fazit - Wichtige Befehle

Um herauszufinden wo die Fehlkonfiguration liegt, sollte man folgende Befehle jeweils auf den Routern durchführen und die Ausgaben dann jeweils auswerten:

=== "Routing Tabelle"
	```cli
    ! Zeigt, ob auch alle Routen da sind
	show ip route
	```
=== "Interface"
	```cli
    ! Zeigt, ob das Interface auch wirklich an ist
	show ip interface brief
	```
=== "Protokolle"
	```cli
    ! Zeigt, ob die richtigen Routen geworben werden
	show ip protocols
	```
=== "running config"
	```cli
    ! Zeigt die vollständige Konfiguration der geworbenen Routen
	show run | b router ospf <Number>
	```

=== "Bandbreite"
	```cli
    ! Zeigt, ob die Bandbreite korrekt gesetzt worden ist
	show int <Interface>
	```