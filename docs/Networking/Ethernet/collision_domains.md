# Collision Domain

Eine **Collision Domain** ist ein Bereich in einem Netzwerk, in dem Datenkollisionen auftreten können. Kollisionen entstehen, wenn mehrere Geräte gleichzeitig Daten über dasselbe Medium senden.

## Beispiel

Stelle dir ein gemeinsames Gespräch in einem Raum vor:
- Wenn zwei Personen gleichzeitig sprechen, kann keiner die andere Person klar verstehen – das ist eine **Kollision**.
- Um das Problem zu lösen, warten Menschen normalerweise, bis die andere Person fertig gesprochen hat – das wäre vergleichbar mit **CSMA/CD**.
- Wenn die Personen sich organisieren und die Redezeiten im Voraus abstimmen, wäre das ähnlich wie **CSMA/CA**.

## Kollisionsdomänen in Netzwerken

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

## Fazit

- **Hubs → große Collision Domain** → hohe Wahrscheinlichkeit für Kollisionen.
- **Switches → getrennte Collision Domains** → verbesserte Netzwerkperformance.
- **Router → Trennung von Collision und Broadcast Domains** → beste Kontrolle über das Netzwerk.
