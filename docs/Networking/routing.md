# Routing

- Dient dazu, den besten Weg von einem Netz in ein anderes Netz zu finden.
- Findet auf Layer 3 im OSI-Modell statt.
- Router kennt standartmäßig nur die Netze die direkt an ihn angeschlossen sind.
- Router entscheiden auf Grund ihrer Routingtabelle und der Destination IP wohin das Paket weitergeleitet wird.
- Wenn ein Router ein Paket bekommt, dessen Zielnetz nicht in seiner Routingtabelle steht, wirft er es weg.
- Routen zu entfernten (nicht direkt angeschlossenen) Netzen müssen in die Routingtabelle eingetragen werden (statisch von Hand oder dynamisch von Routingprotokollen wie z. B. RIP oder OSPF)

## Route Selection (Route Preference)

Der Router berechnet normalerweise die beste Route anhand von:

1. Prefix Length (smallest subnet preferred), e.g. a /32 is preferred over a /30
2. Highest Administrative Distance, e.g. OSPF routes are preferred over RIP routes
3. Lowest Metric, e.g. OSPF routes with metric of 100 are preferred over the routes that have 200 metric cost

### Typ (Herkunft der Route)

Typ der Route mit der jeweiligen Administrative Distance in Cisco:

| Kürzel | **Route Source**                         | **Admin Distance** |
| ------ | ---------------------------------------- | ------------------ |
| C      | Connected (direkt angeschlossen)         | 0                  |
| S      | Static (manuell eingetragen)             | 1                  |
| E      | EIGRP (dynamisch)                        | 90                 |
| O      | OSPF (dynamisch)                         | 110                |
| i      | IS-IS (dynamisch)                        | 115                |
| R      | RIP (dynamisch)                          | 120                |
| iB     | iBGP (dynamisch)                         | 200                |
|        | **Unreachable (nicht vertrauenswürdig)** | **255**            |

### Zielnetz mit entsprechender Subnetzmaske

- Netz! Nicht einzelne Hosts!

### Ausgangsport

Der Ausgangsport kann, muss allerdings nicht in der Routingtabelle stehen.

### next Hop

- IP der nächsten Routerschnittstelle (anderer Router) auf dem Weg zum Ziel
- Bei direkten Routen kann man den next Hop auslassen, wenn der Ausgangsport eingetragen ist

### Metrik

Ganze Zahl >= 0, die angibt, wie gut die Route ist (kleinere Zahl -> besser)

### Beispiel einer Routingtabelle

=== "IPv4"
    | Typ | Zielnetz    | Subnetzmaske  | Ausgangsport   | next Hop     | Metrik |
    | --- | ----------- | ------------- | -------------- | ------------ | ------ |
    | C   | 192.168.1.0 | 255.255.255.0 | fa0/0          | -            | 0      |
    | S   | 192.168.3.0 | 255.255.255.0 | fa1/0          | 192.168.2.2  | 1      |
    | S   | 0.0.0.0     | 0.0.0.0       | fa1/1          | 82.137.5.9   | 1      |

=== "IPv6"
    | Typ | Zielnetz        | Ausgangsport | Next Hop             | Metrik |
    | --- | --------------- | ------------ | -------------------- | ------ |
    | C   | 2001:db8:1::/64 | fa0/0        | -                    | 0      |
    | S   | 2001:db8:3::/64 | fa1/0        | 2001:db8:2::2        | 1      |
    | S   | ::/0            | fa1/1        | 2001:4860:4860::8888 | 1      |

!!! info
    Letzte Route ist die Default Route. Dort steht normalerweise meistens nur der Ausgangsport oder nur der next Hop. Beides, Ausgangsport und next Hop in der Default Route gilt als eher ungewöhnlich.

Abhilfe zur Default Route: Alles, was nicht in der Routingtabelle steht, schicke an next hop,...  
Die Reihenfolge der Einträge ist egal.

## Routing-Loops

Ein Routing-Loop entsteht, **wenn zwei (oder mehr) Router sich gegenseitig Pakete zurückschicken**, weil:

- Sie eine Route zum Ziel über den jeweils anderen Router sehen
		- Beispiel bei Latenzen im dynamischen Routing wie RIP:
				- Ein Link fällt aus, aber durch die **30-Sekunden-Update-Intervalle** weiß ein Router noch nichts davon.
				- Er denkt weiterhin, dass der andere Router den Weg kennt – dieser jedoch schickt es wieder zurück.
				- Das führt zu einem **temporären Routing-Loop**, bis die Routing-Tabellen beim nächsten Update **neu berechnet werden**.
- Oder durch falsch konfigurierte statische Routen ohne gültigen Exit
		- Beispielhafter Loop bei Fehlkonfiguration:
				1. Router A: Route zu `10.1.1.0/24` via Router B
				2. Router B: Route zu `10.1.1.0/24` via Router A → Ergebnis: Endloses Hin und Her

!!! info
	Ein Paket, dessen Ziel die Loopback-Adresse ist, wird vom Router selbst verarbeitet. Es wird nicht weitergeleitet, deswegen können auch keine Loops entstehen.

## Default Route

### Default Gateway vs Default Route

|Begriff|Wer nutzt das?|Bedeutung|
|---|---|---|
|**Default Gateway**|**Endgerät (z. B. PC)**|Der Router, an den ein Gerät **alle Pakete für unbekannte Ziele** schickt.|
|**Default Route**|**Router**|Eine Regel in der Routing-Tabelle: "**Wenn Zielnetz nicht bekannt → dahin!**"|

**Gateway of last resort** (in Windows) = **Default Geteway**

> [!CAUTION]
> **Default Gateway** ist das für den **Client**,  
**Default Route** ist das für den **Router** –  
**beide erfüllen aber denselben Zweck**: Pakete für unbekannte Ziele weiterzuleiten.

### Default Route einrichten

```cli
ip route 0.0.0.0 0.0.0.0 <Routers IP>
```

Alle Netze die in der Routing Tabelle nicht gefunden werden, werden vom Router über den ISP-Provider in Richtung Internet weitergeleitet.
