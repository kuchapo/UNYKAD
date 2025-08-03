# OSPF – Open Shortest Path First

## Ermittlung der OSPF Nachbarn

### Hello Protocol

Das Hello-Protokoll wird bei Routing-Protokollen wie OSPF verwendet, um **Nachbar-Router (Neighbors)** zu erkennen und Verbindungen in Form von Nachbarschaften (Adjacencies) aufzubauen.  
Dafür senden Router in regelmäßigen Abständen sogenannte Hello Messages – aber **nur auf den Interfaces**, auf denen OSPF **aktiviert** wurde.

Das Hello-Paket beinhaltet:

| **Feld**                 | **Beispiel**       | **Bedeutung**                                                                                                  |
| ------------------------ | ------------------ | -------------------------------------------------------------------------------------------------------------- |
| Mask                     | 255.255.255.0      | dient zum Abgleich, ob die OSPF-Router im gleichen Subnetz arbeiten                                            |
| Hello Interval           | ~10 sec or ~30 sec | Zeitintervall, in dem OSPF-Router regelmäßig Hello-Pakete senden (Standard 10 Sek., bei NBMA Standard 30 Sek.) |
| Dead Interval            | 4× Hello Interval  | Zeitspanne, nach der ein Nachbar als tot gilt, wenn keine Hello-Pakete eintreffen                              |
| Area ID                  | 0                  | Identifiziert eine OSPF-Area; Area 0 ist die Backbone-Area                                                     |
| Authentication (if used) | --                 | Optionaler Schutzmechanismus zur Sicherung von OSPF-Nachbarschaften                                            |
| Stub area flag           | none               | Gibt an, ob die Area ein Stub ist (weniger Routinginformationen nötig)                                         |
| MTU size                 | varies             | Maximale Paketgröße, die auf dem Interface übertragen werden kann                                              |

#### OSPF-Nachbarn anzeigen

Der folgende Befehl zeigt die OSPF-Nachbarn, welche durch das Hello Protocol erfasst worden sind:

```cli
show ip ospf neighbor
```

!!! info
	Dieser Befehl zeigt ebenso die States von den Routern an und welche Art von Router direkt verbunden sind (DR, BDR & DROTHER).

### Router ID

- kann ein konfigurierter Wert sein
- wenn nicht konfiguriert, dann wird die höchste IP auf dem Loopback Interface genommen
- wenn kein Loopback Interface konfiguriert ist, dann wird die höchste IP-Adresse aus den aktiven Interfaces ausgewählt, unabhängig davon ob OSPF auf dem Interface aktiv ist oder nicht

Die Router ID wird **beim Start des OSPF-Prozesses** (z. B. `router ospf 1`) und beim Neustart des Routers bestimmt. Sie kann mit dem Befehl `show ip protocols` eingesehen werden. Wichtig: Die Router ID ist keine IPv4-Adresse, sie wird also nicht geroutet!

Die Router ID wird benutzt, um nach dem Austausch der Hello Messages die Neighbor Table zu bilden.

### Aufbau der Neighbor Table

| **Feld**         | **Beispiel** | **Bedeutung**                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Neighbor ID      | 192.168.10.1 | Die **Router ID** (keine IPv4-Adresse -> nicht routebar, obwohl im IPv4-Format) des Nachbarrouters im OSPF-Netzwerk                                                                                                                                                                                                                                                                                                                                                                 |
| State            | Init         | Aktueller Zustand der OSPF-Nachbarschaft.<br>OSPF kennt 8 Zustände:<br>• **Down** – keine Nachbarschaft<br>• **Attempt** – nur bei NBMA-Netzen<br>• **Init** – erstes Hello empfangen, ohne eigene lokale Router ID<br>• **2-Way** – Antwort auf Init, mit eigener lokaler Router ID<br>• **ExStart** – Aushandlung wer Master ist<br>• **Exchange** – LSDB-Austausch beginnt durch DBD-Pakete<br>• **Loading** – LSA-Austausch<br>• **Full** – Nachbarschaft vollständig aufgebaut |
| Dead Time        | 40 sec       | Zeitspanne, bis ein Nachbar als **nicht erreichbar** gilt, wenn keine Hello-Pakete empfangen werden. Standard: 4 × Hello-Intervall                                                                                                                                                                                                                                                                                                                                                  |
| Next Hop Address | 172.16.0.2   | IP-Adresse, zu der das nächste Paket für das Zielnetzwerk weitergeleitet wird (vom lokalen Router gesehen)                                                                                                                                                                                                                                                                                                                                                                          |
| Exit Interface   | F0/1         | Das **lokale Interface**, über das der Router den OSPF-Nachbarn erreicht (z. B. `FastEthernet0/1`)                                                                                                                                                                                                                                                                                                                                                                                  |

## LSA – Link State Advertisement

Ein **Link State Advertisement (LSA)** ist eine strukturierte Nachricht, mit der OSPF-Router ihre Verbindungen (Links), Netzwerke und Kosten (Metriken) an andere Router in derselben Area mitteilen. Alle empfangenen LSAs werden in der **Link-State Database (LSDB)** gespeichert und dienen als Grundlage für die Berechnung der Routing-Tabelle.

### Aufbau & Zweck

| **Feld**           | **Beispiel**     | **Bedeutung**                                                                                         |
| ------------------ | ---------------- | ------------------------------------------------------------------------------------------------------- |
| LSA Type           | 1 (Router-LSA)   | Bestimmt, welche Art von Information das LSA enthält                                                    |
| Advertising Router | 1.1.1.1          | Die Router-ID des Routers, der dieses LSA erzeugt hat                                                   |
| Link State ID      | 10.0.0.0         | Meist die IP-Adresse des Netzwerks oder des Links                                                       |
| **Link Mask**      | 255.255.255.0    | Die Subnetzmaske des Interfaces – wichtig zur Adressierung im Netzwerk                                 |
| Sequence Number    | 0x8000002e       | Zur Versionierung – höhere Zahl = aktuellere Information                                                |
| Age                | 360              | Gibt an, wie alt das LSA ist; nach 1800 s wird es neu geflutet                                          |
| Metric             | 10               | OSPF-Kosten (Cost) zu einem Netzwerk über diesen Link                                                   |

### Wichtige LSA-Typen

| **Typ** | **Bezeichnung** | **Beschreibung**                                                                                 |
| ------: | --------------- | ------------------------------------------------------------------------------------------------ |
|       1 | Router-LSA      | Beschreibt alle Verbindungen und Kosten                                                          |
|       2 | Network-LSA     | Beschreibt Router in einem Broadcast Netzwerk (wie zum Beispiel Ethernet)                        |
|       3 | Summary-LSA     | Verbindet mehrere Areas – enthält Netzinformationen aus anderen Areas (z. B. Area Border Router) |
|     ... | weitere LSAs    | Es gibt viele weitere LSA-Typen                                                                  |

---

## LSDB vs. Routing-Tabelle

| **Merkmal**         | **LSDB (Link-State Database)**                                                 | **Routing-Tabelle**                                                |
|---------------------|--------------------------------------------------------------------------------|--------------------------------------------------------------------|
| Zweck               | Enthält die komplette Netzwerktopologie innerhalb der OSPF-Area                | Zeigt nur die besten Pfade zu Zielnetzwerken                      |
| Inhalt              | Alle empfangenen LSAs (Rohdaten)                                               | Zielnetzwerk, Next Hop, Cost, Exit-Interface                       |
| Verwendung          | Basis für SPF-Berechnung (Dijkstra-Algorithmus)                                | Grundlage für die tatsächliche Weiterleitung von IP-Paketen       |
| Befehl (Cisco)      | `show ip ospf database`                                                        | `show ip route`                                                    |

---

## SPF – Shortest Path First (Dijkstra)

OSPF verwendet den SPF-Algorithmus (Dijkstra), um aus der LSDB die besten Pfade zu allen Zielnetzwerken zu berechnen. Die Ergebnisse werden in die Routing-Tabelle geschrieben.

### Ablauf:

1. LSAs werden empfangen und gespeichert (LSDB)
2. SPF-Berechnung läuft bei:
    - Initialem Start
    - Änderungen im Netzwerk (neue LSAs)
3. Ergebnis ist eine aktualisierte Routing-Tabelle mit den besten Pfaden

---

## Merksätze

- **Hello-Pakete erkennen Nachbarn** – nicht LSAs!
- **LSAs zeigen die Topologie** – also wer ist wo und wie verbunden
- **Die LSDB ist die Karte** – die Routing-Tabelle ist die Navigation

## LSU – Link State Update

Ein **LSU (Link State Update)** ist ein OSPF-Paket vom Typ 4, das einen oder mehrere **LSAs (Link State Advertisements)** enthält. Während LSAs die eigentlichen Informationen über die Netzwerktopologie enthalten, ist das LSU das **Transportmittel**, mit dem diese LSAs zwischen Routern übertragen werden.

### Zusammenhang zwischen LSU und LSA

| **Begriff** | **Bedeutung**                                                    |
|-------------|-------------------------------------------------------------------|
| **LSA**     | Enthält Informationen über Netzwerke, Kosten, Nachbarn usw.      |
| **LSU**     | Pakettyp, der einen oder mehrere LSAs zu anderen Routern sendet  |

LSUs werden gesendet bei:

- Aufbau einer neuen Nachbarschaft (z. B. während „Loading“)
- Änderungen im Netzwerk (z. B. Interface down)
- regelmäßig zum Aktualisieren der LSAs (Refresh, Standard alle 30 Minuten)

## Message Types

| **Typ** | **Name**                          | **Verwendung**                                      |
| ------: | --------------------------------- | --------------------------------------------------- |
|       1 | Hello                             | Nachbarn erkennen und Nachbarschaften aufbauen      |
|       2 | Database Description (DBD)        | Überblick über LSDB beim Nachbarschaftsaufbau       |
|       3 | Link-State Request (LSR)          | Anforderung fehlender LSAs vom Nachbarn             |
|       4 | Link-State Update (LSU)           | Überträgt LSAs zwischen OSPF-Routern                |
|       5 | Link-State Acknowledgment (LSAck) | Bestätigung, dass LSAs erfolgreich empfangen wurden |

Der OSPF-Prozess beim Aufbau einer OSPF-Nachbarschaft verläuft von Typ 1 aufsteigend.

---

## Merksatz

> 📨 **LSA** = die Info selbst  
> 📦 **LSU** = das Paket, das diese Info verschickt


## LSDB – Link State Database

### Aufbau

| Feld          | Beispiel   | Bedeutung                                                            |
| ------------- | ---------- | -------------------------------------------------------------------- |
| Router ID     | 172.16.0.1 | Der Router, der diesen LSA erzeugt hat.                              |
| Link State ID | 10.0.0.0   | Zielnetz oder Router-ID, je nach LSA-Typ.                            |
| Link Mask     | /24        | Subnetzmaske des Zielnetzes (bei Netz-LSAs, z. B. Typ 1 oder Typ 3). |
| LSA Type      | 1          |                                                                      |

## Designated Router (DR)

DR und BDR gibt es in OSPF nur auf Mehrzugriffsnetzen (Broadcast / NBMA) wie Ethernet, nicht auf Point-to-Point-Links. Ziel: Reduzierung der Anzahl der Nachbarschaften und Erhöhung der Effizienz.

- **Aufgabe:**
      - Der DR ist dafür verantwortlich, Link-State-Advertisements (LSAs) zu sammeln und an alle anderen OSPF-Router weiterzugeben.
      - Er agiert quasi als „Sprecher“ des Netzwerks: Alle Router müssen nicht mit allen anderen direkt kommunizieren, sondern nur mit dem DR (und BDR).

- **Warum gibt es einen DR?**
    - In einem Netzwerk mit z. B. 10 Routern gäbe es ohne DR 45 Nachbarschaften (n * (n-1)/2).
    	- **Jeder Router muss mit allen anderen Routern verbunden sein** → also `n - 1` Verbindungen pro Router.
    	- **Jede Verbindung wird doppelt gezählt** (z. B. R1↔️R2 und R2↔️R1).    
    	- Deshalb teilen wir durch 2, um die echten (einmaligen) Verbindungen zu zählen.
    	- Ergebnis: **`n * (n-1) / 2` = Anzahl eindeutiger Nachbarschaften im Full-Mesh.**
    - Mit DR reduziert sich das drastisch, weil alle Router nur mit dem DR sprechen müssen.

- **DR wird bestimmt über:**
    - Höchste OSPF-Router-ID (falls Priorität gleich)
    - Priorität (`ip ospf priority`) kann konfiguriert werden, Standardwert ist 1.
        - Wenn auf jedem Router auf 0, dann darf keiner DR bzw. BDR werden

### Backup Designated Router (BDR)

Der **Backup Designated Router (BDR)** ist ein Ersatz für den DR, falls dieser ausfällt. Er übernimmt automatisch die Rolle des DR, falls dieser nicht mehr erreichbar ist.

## Passive Interface

- bekommen keine Hello Messages
- reduziert unnötigen Traffic, z. B. bei den PC-Clients

```cli
router ospf <process-id>
passive-interface <interface-name>
```

## Wild Card Mask

= inverse Subnet Mask

Alles was eine `1` ist wird eine `0` & alles was eine `0` ist wird zu einer `1` bei einer binären Subnetzmaske.

### Beispiel einer Umrechnung

Wenn /30 die Subnetzmaske ist:

=== "dezimal"
	255.255.255.255 (/32)  
	-255.255.255.252 (/30)  
	=0.0.0.3

=== "binär"
	1111 1111 . 1111 1111 . 1111 1111 . 1111 1111  
	-1111 1111 . 1111 1111 . 1111 1111 . 1111 1100  
	=0000 0000 . 0000 0000 . 0000 0000 . 0000 0011


## Beispiel

![OSPF Beispiel](assets/ipv4_ospf.drawio.svg)

### Router A konfigurieren

OSPF auf dem Router aktivieren:

```cli
router ospf 10
```

Man kann auf einem Router mehrere OSPF Instanzen am Laufen haben. Die hintere Zahl `10` ist lokal auf dem Router und sie verlässt nicht den Router.

```cli
network 10.0.0.0 0.0.0.255 area 0
network 172.16.0.0 0.0.0.3 area 0
```

!!! info
	Wenn RIP ebenfalls eingerichtet ist, dann wird RIP weiterhin noch mit dem Befehl `show ip route` angezeigt werden. Der Befehl `show ip protocols` zeigt an, dass beide Protokolle aktuell aktiv sind.

### Router B konfigurieren

```cli
router ospf 10
network 192.168.10.0 0.0.0.255 area 0
network 172.16.0.0 0.0.0.3 area 0
```

!!! info
	Bei aktivem RIP würden beide Protokolle (RIP und OSPF) parallel arbeiten und Routen mit ihren jeweiligen Nachbarn austauschen. In der globalen Routing-Tabelle (`show ip route`) erscheinen jedoch ausschließlich die Routen, die vom Router aufgrund der besten Metrik und der niedrigeren administrativen Distanz bevorzugt werden — hier also die OSPF-Routen.

## Kurze OSPF Zusammenfassung

OSPF ist ein Link-State-Routing-Protokoll, bei dem Router Nachbarschaften über das **Hello-Protokoll** aufbauen. Jeder Router pflegt dabei eine eigene **LSDB (Link-State Database)** innerhalb seiner Area. Jeden Eintrag in dieser LSDB nennt man **LSA (Link-State Advertisements)**. Wenn LSAs verschickt werden, dann nennt man diese Pakete **LSUs (Link-State Update)**.

Die **Router-ID** kann **manuell festgelegt** werden. Wenn sie nicht manuell festgelegt wird, wählt der Router automatisch die höchste IP-Adresse eines vorhandenen **Loopback-Interfaces**. Falls kein Loopback existiert, wird die höchste IP-Adresse eines **aktiven Interfaces** verwendet.

Die Router-ID wird im IPv4-Format dargestellt und dient nur als **eindeutige Kennung** für den Router innerhalb von OSPF. Nach der Wahl der ID spielt es keine Rolle mehr, ob dieses Interface noch existiert oder erreichbar ist.

Bei der Wahl des **Designated Router (DR)** und **Backup Designated Router (BDR)** entscheidet zuerst die **OSPF Priority** (höchste gewinnt), bei gleicher Priority die **höchste Router-ID**. Ein DR & BDR wird pro Broadcast- oder NBMA-Netzwerksegment gewählt. Wenn ein DR bzw. BDR nicht erreichbar ist, wird automatisch ein neuer bestimmt.

Ein DROTHER-Router ist in OSPF ein Router, der in einem Netzwerkssegment (Broadcast Domain) weder DR noch BDR ist und LSAs ausschließlich über den DR bzw. BDR austauscht. In einem anderen Segment kann derselbe Router jedoch durchaus die Rolle eines DR oder BDR übernehmen.

Der DR koordiniert den LSA-Austausch, um Netzwerkverkehr zu minimieren. Alle DROTHER-Router senden ihre LSAs nur an den DR, statt sich gegenseitig alles zu schicken. Der BDR ist die **Sicherheitskopie** des DR und wird nur aktiv, wenn der DR ausfällt und der BDR zum neuen DR wird.

Falls der ausgefallene DR dann wieder online ist, wird dieser allerdings zu einem DROTHER-Router. Das liegt daran, dass wenn ein neuer Router online kommt, es nicht erneut zu einer DR/BDR-Wahl kommt. Es kommt nur zu einer DR/BDR-Wahl, wenn der DR bzw. BDR nicht erreichbar ist.

!!! info
	Da ein OSPF-Router in jedem Netzwerksegment (z. B. pro Interface) separat eine DR/BDR-Wahl durchläuft, kann ein und derselbe Router gleichzeitig in einem Segment als DROTHER und in einem anderen Segment als DR oder BDR fungieren. Ein OSPF-Router kann daher in unterschiedlichen Segmenten gleichzeitig als DR, BDR und DROTHER fungieren.