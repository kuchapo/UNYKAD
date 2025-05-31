# Collision and Broadcast Domains

## Collision Domains

Eine **Collision Domain** ist ein Bereich in einem Netzwerk, in dem Datenkollisionen auftreten können. Kollisionen entstehen, wenn mehrere Geräte gleichzeitig Daten über dasselbe Medium senden.

### Beispiel

Bei einem gemeinsamen Gespräch in einem Raum:

- Wenn zwei Personen gleichzeitig sprechen, kann keiner die andere Person klar verstehen – das ist eine **Kollision**.
- Um das Problem zu lösen, warten Menschen normalerweise, bis die andere Person fertig gesprochen hat – das wäre vergleichbar mit **CSMA/CD**.
- Wenn die Personen sich organisieren und die Redezeiten im Voraus abstimmen, wäre das ähnlich wie **CSMA/CA**.

### Kollisionsdomänen in Netzwerken

**Hub-basiertes Netzwerk**:

- Alle angeschlossenen Geräte befinden sich in einer gemeinsamen Collision Domain, weil ein Hub Daten an jedes angeschlossene Gerät weiterleitet.
- Wenn ein Gerät sendet, müssen alle anderen warten – das kann zu vielen Kollisionen führen.

**Switch-basiertes Netzwerk**:

- Switches teilen das Netzwerk in mehrere separate Collision Domains.
- Jedes Gerät, das direkt mit einem Switch-Port verbunden ist, hat seine eigene Collision Domain.
- Dadurch kann jedes Gerät unabhängig senden und empfangen, ohne dass andere gestört werden.

**Router-basierte Netzwerke**:

- Router trennen nicht nur Kollisionsdomänen, sondern auch **Broadcast Domains** (Bereiche, in denen Broadcast-Nachrichten gesendet werden).
- Das sorgt für eine sauberere und effizientere Netzwerkkonfiguration.

### Fazit

- **Hubs → große Collision Domain** → hohe Wahrscheinlichkeit für Kollisionen.
- **Switches → getrennte Collision Domains** → verbesserte Netzwerkperformance.
- **Router → Trennung von Collision und Broadcast Domains** → beste Kontrolle über das Netzwerk.

---

## Broadcast Domains

Eine **Broadcast Domain** ist ein Bereich im Netzwerk, in dem ein **Broadcast-Paket von allen Geräten empfangen wird**. Broadcasts sind spezielle Nachrichten, die an alle Geräte in einem Netzwerksegment gesendet werden.

### Beispiel

Vergleichen wir es mit einem Lautsprecher in einem Raum:

- Eine Person spricht über ein Mikrofon mit Lautsprechern.
- Alle Personen im Raum hören die Nachricht, egal ob sie direkt angesprochen wurden oder nicht.
- Das ist vergleichbar mit einem **Netzwerk-Broadcast**, bei dem alle Geräte eine Nachricht empfangen.

### Broadcast-Domänen in Netzwerken

**Hub- oder Switch-basierte Netzwerke**:

- Alle Geräte im selben **VLAN oder Netzwerksegment** gehören zur **gleichen Broadcast Domain**.
- Ein Broadcast-Paket, wie eine **ARP-Anfrage**, wird an alle Geräte innerhalb dieser Domain gesendet.
- Switches **reduzieren keine Broadcast-Domains**, sondern leiten Broadcasts weiter.

**Router-basierte Netzwerke**:

- **Router trennen Broadcast-Domains**.
- Ein Broadcast innerhalb eines Netzwerks **wird nicht über den Router weitergeleitet** – so wird Broadcast-Verkehr auf kleine Segmente begrenzt.
- Dadurch wird unnötiger Netzwerkverkehr reduziert und die Effizienz verbessert.

### Fazit

- **Hubs & Switches → gleiche Broadcast Domain** → Broadcasts werden an alle Ports gesendet.
- **Router → trennen Broadcast Domains** → Broadcasts bleiben innerhalb eines Netzwerks und werden nicht weitergeleitet.

---

## Vergleich: Collision Domain vs. Broadcast Domain

| **Merkmal**         | **Collision Domain**                     | **Broadcast Domain**                  |
|---------------------|--------------------------------------|-------------------------------------|
| **Definition**      | Bereich, in dem Datenkollisionen möglich sind | Bereich, in dem Broadcast-Nachrichten empfangen werden |
| **Beeinflusst von** | Hubs & Switches (Layer 2)            | Switches & Router (Layer 2 & 3)    |
| **Begrenzung**      | Switches trennen Collision Domains   | Router trennen Broadcast Domains  |
| **Kommunikation**   | Geräte teilen sich das Medium (bei Hubs) | Geräte hören Broadcasts im selben VLAN oder Netzsegment |
| **Effizienz**       | Mehr Collision Domains = weniger Kollisionen | Mehr Broadcast Domains = weniger Netzwerkbelastung |
