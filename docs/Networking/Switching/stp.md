# Spanning Tree Protocol (STP)

## Was ist STP?

Das **Spanning Tree Protocol (STP)** ist ein Netzwerkprotokoll, das Loops in geswitchten Netzwerken verhindert. Es sorgt dafür, dass es keine Endlosschleifen gibt, wenn mehrere Switches miteinander verbunden sind.

## Wie funktioniert STP?

STP arbeitet nach folgendem Prinzip:

1. **Bestimmung des Root-Switches**  
   - Einer der Switches wird als **Root Bridge** (Wurzel-Switch) gewählt.  
   - Der Switch mit der niedrigsten Bridge-ID wird Root.

2. **Berechnung der besten Pfade**  
   - Jeder Switch bestimmt den **kürzesten Weg** zur Root Bridge.  
   - Die Ports mit den besten Pfaden bleiben **aktiv**.

3. **Blockierung redundanter Links**  
   - Verbindungen, die zu Loops führen könnten, werden **deaktiviert** (in den „Blocking“-Zustand versetzt).  
   - Nur die beste Verbindung bleibt offen.

4. **Neuberechnung bei Änderungen**  
   - Falls ein Switch oder eine Verbindung ausfällt, berechnet STP die Netzwerkstruktur neu.  
   - Blockierte Verbindungen können wieder aktiviert werden.

## Vorteile von STP

✅ Verhindert **Loops** in geswitchten Netzwerken  
✅ **Erhöht Stabilität** durch automatische Anpassung  
✅ **Redundanz** bleibt erhalten, ohne das Netzwerk zu stören  

## Varianten von STP

- **RSTP (Rapid Spanning Tree Protocol)** → Schnellere Reaktionszeit  
- **PVSTP (Per VLAN STP)** → Separate STP-Instanzen pro VLAN  
- **PVRSTP (Per VLAN RSTP)** → Kombination aus PVSTP und RSTP  

STP ist essenziell für stabilen **Layer 2-Switching-Betrieb** in Netzwerken.
