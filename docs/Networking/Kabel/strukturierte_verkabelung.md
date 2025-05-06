# Strukturierte Verkabelung

- einheitlicher Netzaufbau für eine zukunftsorientierte und anwendungsunabhängige Netzwerkinfrastruktur
- Skalierbarkeit (Netze können erweitert werden ohne langsamer zu werden)
- Ausfallsicherheit durch Sterntopologie
- genormter Aufbau des Netzes (EN 50173-1 von 2003)

![Verkabelung](./assets/verkabelung.drawio.svg)

## Arten der Verkabelung

### Geländeverkabelung (Primärverkabelung, Campusverkabelung)

- verbindet Gebäude untereinander
- Verbindung vom Standortverteiler zu den Gebäudeverteilern
- Glasfaser, da unempfindlich gegen Blitzeinschläge und Potentialunterschiede zwischen den
- Gebäuden. Außerdem bietet Glasfaser eine hohe Bandbreite und Leitungslängen von mehreren Kilometern

### Gebäudeverkabelung (Sekundärverkabelung, Steigbereichverkabelung, Vertikalverkabelung)

- verbindet die Stockwerke eines Gebäudes
- Verbindung vom Gebäudeverteiler zu den Etagenverteilern
- Glasfaser- oder Kupferkabel

### Etagenverkabelung (Tertiärverkabelung, Etagenverkabelung, Horizontalverkabelung)

- Verbindung von den Etagenverteilern zu den Anschlussdosen
- meistens Kupferkabel, selten Glasfaserkabel
- maximale Länge einer Verbindung mit Kupferkabel 100m (Patchkabel zum Endgerät – Anschlussdose -fest verlegtes Kabel – Patchpanel – Patchkabel zum Etagenverteiler)
- beim Collapsed Backbone wird auf die Etagenverteiler verzichtet

---

## Verteilergeräte

Standortverteiler: Core-Switches, Layer3-Switches, Router  
Gebäudeverteiler: Layer3-Switches, Router, VLAN-fähige Layer2 Switches  
Etagenverteiler: Layer3-Switches, VLAN-fähige Layer2 Switches, Layer2 Switches
