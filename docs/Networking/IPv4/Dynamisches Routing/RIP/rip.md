# Routing Information Protocol (RIP)

**RIP** ist ein einfaches **Routing-Protokoll**, welches Routern hilft, den Weg zu Netzwerken zu finden.  
Es benutzt als **Metrik** die **Anzahl der Hops** (also wie viele Router ein Paket durchqueren muss).

→ **Je weniger Hops**, desto besser der Weg.

## RIP aktivieren

`RIPv1` wird im **Global Configuration Mode** mit folgendem Befehl aktiviert:

```cli
router rip
```

!!! info
	`RIPv1` unterstützt **kein CIDR** (Classless Inter-Domain Routing), da es ein sogenanntes **klassenbasiertes Routing-Protokoll** (classful) ist. Das bedeutet, dass **keine Subnetzmasken** berücksichtigt werden – es geht immer von der Standardnetzmaske der IP-Adresse aus (z. B. /8, /16, /24). Die **Subnetzmaske wird ignoriert**, und stattdessen die **Standardklasse der IP-Adresse** (A, B, C) verwendet, um Netzwerke zu _bewerben_. Dies kann zu Routing-Problemen führen – deshalb verwendet man heute in der Regel das deutlich flexiblere **klassenlose Routing-Protokoll** (classless) `RIPv2`.

Um `RIPv2` zu aktivieren, ergänzt man:

```cli
version 2
```

Zusätzlich sollte man das Feature `auto-summary` deaktivieren:

```cli
no auto-summary
```

!!! info
	Wenn `auto-summary` aktiviert ist, **rundet der Router gelernte Netze auf die Standardklasse auf**.  
	**Beispiel**: Ein Router lernt `192.168.1.0/24`, aber mit aktiviertem `auto-summary` sendet er es als `192.168.0.0/16`.  
	Das führt oft zu **falschen oder ungenauen Routing-Informationen** – deshalb wird `auto-summary` heute meist **deaktiviert**. 

## Network Advertising

Nur Netzwerke, **deren Interfaces eingeschaltet (up)** sind und **direkt mit dem Router verbunden** sind, können mit RIP weitergegeben werden.

### Beispiel 1

Ein Router hat 3 Ports mit diesen IPs:

- **Port 1:** `10.10.1.1/30`
- **Port 2:** `10.20.5.1/24`
- **Port 3:** `192.168.1.1/24`

Dann gibt man z. B. ein:

```cli
router rip
version 2
no auto-summary
network 10.10.0.0
```

RIP schaut auf `10.10.0.0` und erkennt:

> „Das ist eine IP aus dem Class-A-Netz `10.0.0.0/8`. Ich aktiviere alle Interfaces mit IPs, die mit `10.` beginnen.“

Ergebnis:

- **Port 1** wird aktiviert ✅
- **Port 2** wird aktiviert ✅
- **Port 3** wird ignoriert ❌ (fällt nicht in den Bereich `10.0.0.0/8`)

Jetzt sagt RIP zu anderen Routern:

> „Ich kenne `10.10.1.0/30` und `10.20.5.0/24` – über mich erreichbar!“

**Diese Subnetze werden advertised** – **nicht** das, was beim `network`-Befehl steht!  
Der `network`-Befehl sagt dem Router nur, welche Interfaces bei RIP mitmachen sollen.

#### Anmerkung

Durch den `network`-Befehl erscheint in der Running Config das **klassenbasierte Netz** `10.0.0.0`, auch wenn ursprünglich `network 10.10.0.0` eingegeben wurde.

Befehl, um die Konfiguration anzeigen zu lassen:

```cli
do show run | begin router rip
```

#### Anzeigen, was advertised wird

Mit folgendem Befehl kann man die tatsächlich beworbenen Netze sehen:

```cli
show ip rip database
```

#### Testen, ob RIP aktiv ist

Der folgende Befehl listet alle aktiven Protokolle auf. Er zeigt ebenso die allgemeine Konfiguration an. Der Befehl `show ip rip database` zeigt genauere Informationen an zu den wirklich beworbenen Netzen.

```cli
show ip protocols
```

## Beispiel 2

![RIP](assets/RIP.drawio.svg)

### Router A Konfiguration

```cli
router rip
ver 2
no auto-summary
network 10.10.0.0
network 192.168.0.0
```

Der Befehl `do show run | b router rip` zeigt nun `network 10.0.0.0` an.

### Router B Konfiguration

```cli
router rip
ver 2
no auto-summary
network 10.0.0.0
network 192.168.10.0
```

Wenn man nun `show ip route` auf Router B ausführt, sollte folgende Tabelle zu sehen sein:

=== "Original"
	
	| Kürzel | Infos zum Zielnetz                                       |
	| ------ | -------------------------------------------------------- |
	| C      | 192.168.10.0/24 is directly connected, FastEthernet0/1   |
	|        | 10.0.0.0/30 is subnetted, 3 subnets                      |
	| C      | 10.10.0.0 is directy connected, Serial0/1/0              |
	| R      | 10.10.0.4 [120/1] via 10.10.0.1, 00:00:03, Serial0/1/0   |
	| C      | 10.10.0.8 is directly connected, Serial0/0/0             |
	| R      | 192.168.0.0 [120/1] via 10.10.0.1, 00:00:03, Serial0/1/0 |

=== "Vereinfacht"
	
	| Kürzel | Zielnetz     | Subnetzmaske | Administrative Distance | Metrik | Via       |
	| ------ | ------------ | ------------ | ----------------------- | ------ | --------- |
	| C      | 192.168.10.0 | /24          | 0                       | --     | --        |
	| C      | 10.10.0.0    | /30          | 0                       | --     | --        |
	| R      | 10.10.4.0    | /30          | 120                     | 1      | 10.10.0.1 |
	| C      | 10.10.8.0    | /30          | 0                       | --     | --        |
	| R      | 192.168.0.0  | /24          | 120                     | 1      | 10.10.0.1 |

Man erkennt, dass Router A seine gesamte Routing Tabelle mit Router B geteilt hat und man erkennt durch die Metrik, dass dass beim RIP-Protokoll jeweils nur ein Hop bis zum Zielnetz benötigt wird.

### Router C Konfiguration

```cli
router rip
ver 2
no auto-summary
network 10.0.0.0
network 192.168.20.0
```

Wenn man nun `show ip route` auf Router B ausführt, sollte folgende Tabelle zu sehen sein:

=== "Original"
	
	| Kürzel | Infos zum Zielnetz                                           |
	| ------ | ------------------------------------------------------------ |
	| R      | 192.168.10.0/24 [120/1] via 10.10.0.9, 00:00:02, Serial0/1/0 |
	| C      | 192.168.20.0/24 is directly connected, FastEthernet0/1       |
	|        | 10.0.0.0/30 is subnetted, 3 subnets                          |
	| R      | 10.10.0.0 [120/1] via 10.10.0.9, 00:00:02, Serial0/1/0       |
	|        | 10.10.0.0 [120/1] via 10.10.0.5, 00:00:20, Serial0/0/0       |
	| C      | 10.10.0.4 is directly connected, Serial0/0/0                 |
	| C      | 10.10.0.8 is directy connected, Serial0/1/0                  |
	| R      | 192.168.0.0/24 [120/1] via 10.10.0.5, 00:00:20, Serial0/0/0  |

=== "Vereinfacht"
	
	| Kürzel | Zielnetz     | Subnetzmaske | Aministrative Distance | Metrik | Via       |
	| ------ | ------------ | ------------ | ---------------------- | ------ | --------- |
	| R      | 192.168.10.0 | /24          | 120                    | 1      | 10.10.0.9 |
	| C      | 192.168.20.0 | /24          | 0                      | --     | --        |
	| R      | 10.10.0.0    | /30          | 120                    | 1      | 10.10.0.9 |
	| R      | 10.10.0.0    | /30          | 120                    | 1      | 10.10.0.5 |
	| C      | 10.10.0.4    | /30          | 0                      | --     | --        |
	| C      | 10.10.0.8    | /30          | 0                      | --     | --        |
	| R      | 192.168.0.0  | /24          | 120                    | 1      | 10.10.0.5 |

Hier erkennt man nun dass ein Kürzel R zwei Mal das identische Zielnetz hat. Hier findet ein Load Balancing statt. Es wird also zuerst über das eine Interface (`10.10.0.9`) Daten geschickt und dann über das andere (`10.10.0.5`).

!!! info
	In RIP hat man nicht die Möglichkeit einen eindeutigen Pfad ins Zielnetz zu wählen, wenn mehrere Routen in dasselbe Zielnetz führen. Das limitiert die Möglichkeiten dieses Protokolls.

## Nachteile

RIP gilt heute als veraltet, da es im Vergleich zu modernen Routing-Protokollen wie OSPF oder EIGRP **langsamer** arbeitet und nur **einfache Routing-Entscheidungen** treffen kann.

Ein großes Manko ist, dass RIP **alle 30 Sekunden seine komplette Routing-Tabelle** an alle Nachbarrouter sendet – **unabhängig davon, ob sich etwas geändert hat oder nicht**.  
Das führt zu:

- **unnötigem Datenverkehr** im Netzwerk  
- **langsamer Reaktion** auf Netzwerkänderungen  
- einem höheren Risiko für **Routing-Loops**

Zwar bietet RIP Mechanismen wie **Split Horizon**, **Route Poisoning** und **Hold-Down-Timer**, um Loops zu verhindern – dennoch ist es gegenüber modernen Protokollen technisch unterlegen.

Zusätzlich unterstützt RIP nur eine **maximale Hop-Anzahl von 15**, was es in größeren Netzwerken **unbrauchbar macht**.

Außerdem nutzt RIP bei mehreren gleichwertigen Routen automatisch **Load Balancing**, ohne dabei gezielt den besten Pfad auszuwählen.
