# VRF (Virtual Routing and Forwarding)

## Kurz erklärt

**VRF** (Virtual Routing and Forwarding) ist eine Technik, mit der man **mehrere getrennte Routing-Tabellen** auf einem einzigen **Router oder Layer-3-Switch** betreiben kann – als hätte man mehrere „virtuelle Router“ auf einem Gerät.

Dabei ist jedes VRF-Netz **komplett isoliert** von den anderen – auch wenn:

- **dieselben IP-Adressbereiche verwendet werden**
- **dieselbe Subnetzmaske**
- **sogar dasselbe Default-Gateway** konfiguriert ist

> VRF trennt also Netzwerke logisch – obwohl sie auf derselben physischen Infrastruktur laufen.

---

## Beispiel: Zwei Kunden im selben Subnetz

| Gerät      | IP-Adresse   | Subnetzmaske  | Gateway     | VRF     |
| ---------- | ------------ | ------------- | ----------- | ------- |
| Kunde A PC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | `VRF-A` |
| Kunde B PC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | `VRF-B` |

Beide Geräte haben **identische IPs, Subnetzmaske und Gateway** – aber befinden sich in **verschiedenen VRFs**.

**Ergebnis**:  
Sie können **nicht miteinander kommunizieren**, da VRF wie eine **logische Trennung** wirkt – so wie bei VLANs auf Layer 2.

---

## Wichtige Merkmale

- Jedes VRF hat seine **eigene Routing-Tabelle**
- Nur **Router und L3-Switches** unterstützen VRF
- Standardmäßig ist jeder Port im **VRF "default"**
- Kommunikation **zwischen VRFs** (Inter-VRF-Routing) ist nur mit spezieller Konfiguration möglich (z. B. Route Leaking)

---

## Arten von VRF

| Art                | Einsatzgebiet       | Besonderheit                          |
| ------------------ | ------------------- | ------------------------------------- |
| **VRF (mit MPLS)** | Service Provider    | Benötigt **MPLS**, unterstützt VPNs   |
| **VRF Lite**       | Unternehmen / lokal | Kein MPLS, einfacher, weit verbreitet |

**VRF Lite** ist ideal für Unternehmensnetzwerke, da es ohne den Einsatz von MPLS funktioniert.

---

## Warum VRF?

- Mehrere Kunden oder Abteilungen isolieren
- Gleiche IP-Adressbereiche mehrfach nutzen
- Netzwerke sicher voneinander trennen

---

## Befehl zur Anzeige der Routing-Tabelle pro VRF

```cli
show ip route vrf <VRF-Name>
```

---

## Fazit

Mit VRF kannst du auf einem einzigen Router mehrere **unabhängige Netzwerke** betreiben – wie mehrere Router in einem Gerät.  
Selbst wenn alles gleich aussieht (IP, Gateway, Subnetz), bleiben sie isoliert.
