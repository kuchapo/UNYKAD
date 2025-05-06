# Spanning Tree Protocol (STP)

Das **Spanning Tree Protocol (STP)** verhindert Netzwerkschleifen in redundanten Verbindungen zwischen Switches, indem es automatisch eine logische Baumstruktur erstellt. Es erkennt doppelte Verbindungen und deaktiviert sie vorübergehend, sodass Pakete nur über eine aktive, störungsfreie Route laufen.

## Das Problem ohne redundante Verbindungen

![Ohne redundante Verbindungen](./assets/ohne_redundante_verbindungen.drawio.svg)

### Single Point of Failure

Ein **Single Point of Failure (SPOF)** ist eine kritische Verbindung oder Komponente, die im Netzwerk ohne Redundanz existiert und deren Ausfall das gesamte System beeinträchtigen kann.

Beispiele:

- **Switches** – Eine einzelne Verbindung ohne Backup
- **Router** – Kein redundanter Gateway-Router
- **Server** – Keine Failover-Lösung für Web-, Datenbank- oder DNS-Server
- **Firewalls** – Nur eine zentrale Firewall ohne Ersatz
- **Stromversorgung** – Kein Backup-Netzteil oder USV
- **Internetanbindung** – Eine einzige Internetverbindung ohne Ausweichmöglichkeit

## Das Problem mit redundanten Verbindungen ohne STP

![Mit redundante Verbindungen](./assets/mit_redundante_verbindungen.drawio.svg)

!!! info
	In der Grafik hätte man genauso gut nur zwei doppelt miteinander verbundene Switches verwenden können.

Bei redundanten Verbindungen ohne STP kann es bei einem Broadcast zu einem Broadcast-Sturm kommen. Dabei entsteht eine Endlosschleife, die das gesamte Netzwerk belastet. Der Sender ist in diesem Beispiel nicht vom Flooding betroffen, wäre es aber, wenn er z. B. mit einem anderen Switch verbunden wäre, der ebenfalls vom Sturm betroffen ist.

## Versionen von STP

| Protokoll/Technologie                              | Netzwerkstandard | Beschreibung                                                                                     |
| -------------------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------ |
| **STP** (Spanning Tree Protocol)                   | IEEE 802.1D      | Standardprotokoll zur Vermeidung von Netzwerkschleifen.                                          |
| **RSTP** (Rapid Spanning Tree Protocol)            | IEEE 802.1w      | Schnellere STP-Variante mit kürzerer Konvergenzzeit.                                             |
| **PVST** (Per VLAN Spanning Tree)                  | Cisco proprietär | Ältere Cisco-Version von STP mit separaten Instanzen pro VLAN.                                   |
| **PVST+** (Per VLAN Spanning Tree Plus)            | Cisco proprietär | Weiterentwicklung von PVST mit 802.1D-Kompatibilität.                                            |
| **PVRSTP** (Per VLAN Rapid Spanning Tree Protocol) | Cisco proprietär | PVST+, aber mit schnelleren STP-Reaktionen durch RSTP.                                           |
| **MSTP** (Multiple Spanning Tree Protocol)         | IEEE 802.1s      | Gruppiert mehrere VLANs in eine einzelne STP-Instanz für bessere Skalierung. (selten vorkommend) |

## Bridge Protocol Data Units (BPDU)

![BPDUs](./assets/bpdus.drawio.svg)

Die Kommunikation bei STP verläuft über sogenannte BPDUs. Sie enthalten immer eine Bridge ID (BID) bestehend aus der Priorität, der MAC-Adresse und der Extended System ID.

## Auswahl der Root-Bridge

![Ohne redundante Verbindungen](./assets/auswahl.drawio.svg)

Niedrigste Priorität gewinnt. Falls alle Prioritäten gleich sind, dann gewinnt der Switch, welcher die kleinste MAC-Adresse hat.

## Metriken für die Berechnung des kürzesten Pfads zur Root-Bridge

![Ohne redundante Verbindungen](./assets/metriken.drawio.svg)

| **Link-Geschwindigkeit** | **Standard-Kostenwert** |
|--------------------------|-------------------------|
| 10 Mbps                  | 100                     |
| 100 Mbps                 | 19                      |
| 1 Gbps                   | 4                       |
| 10 Gbps                  | 2                       |

## Port-Rollen

![Ohne redundante Verbindungen](./assets/portrollen.drawio.svg)

Der Root-Port R wird vergeben, indem geschaut wird, welcher der Ports unter Berücksichtingung der Metrik näher am Router ist.    
(z. B. Metrik-Berechnung bei S4: blaue Strecke 4+4=8, grüne Strecke 19+19=38) -> R auf der blauen Strecke

Die Root-Bridge kann keine Ports im Blocking-Zustand haben. Folglich sind bei der Root-Bridge alle Ports Designated-Ports.

Jetzt können Dedignated-Ports überall ergänzt werden, wo auf der anderen Seite auch ein Root-Port bereits eingetragen ist.

![Ohne redundante Verbindungen](./assets/portrollen1.drawio.svg)

Dort wo auf beiden Seiten keine Root-Ports festgelegt worden waren, muss nun geschaut werden, welcher Pfad von der Root-Bridge aus gesehen, der kürzeste ist. Der wird zum Designated-Port und der andere entsprechend zum Blocking-Port.

### Port-Rollen Übersicht

* Root-Port: Der Port mit dem kürzesten Weg zur Root-Bridge. Jeder Switch (bis auf die Root-Bridge) weist diese Rolle genau einem Port zu.
* Designated-Port: Alle Ports, die den Verkehr weiterleiten, aber keine Root-Ports sind. Allen Ports des Root-Bridge wird diese Rolle zugewiesen.
* Blocking-/Alternate-Ports: Diese Ports lösen die Schleifen auf. Wenn ein Trunk an beiden Enden keine Root-Ports aufweist, nimmt ein Port diese Rolle Rolle ein.

## Kurze Zusammenfassung zur Funktionsweise von STP

- **Root Bridge wählen**: Switch mit niedrigster Bridge-ID (Priorität + MAC-Adresse).
- **Root Ports**: Jeder Switch bestimmt den besten Weg zur Root Bridge.
- **Designated Ports**: Für jedes Segment wird ein bevorzugter Weiterleitungsport festgelegt.
- **Blocking Ports**: Überflüssige Pfade werden deaktiviert, um Schleifen zu vermeiden.
