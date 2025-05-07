# Allgemeines zu Kabeln

## Kabelarten

### Wandkabel

- Werden dauerhaft in Wänden, Kabelkanälen oder Böden verlegt
- Meist starr (massive Adern) → schwer biegsam, aber langlebig
- Für feste Netzwerkinstallationen gedacht (z. B. vom Patchpanel zur Netzwerkdose)
- Nicht zum häufigen Umstecken geeignet

Beispiel: Von einem Switch im Serverschrank durch die Wand zur Netzwerkdose im Büro

### Patchkabel

- Flexibel und beweglich, mit feindrähtigen Adern
- Wird genutzt für kurze Verbindungen:
	- PC ↔ Dose,
    - Switch ↔ Patchpanel,
    - Router ↔ Modem
- Mit fertigen RJ45-Steckern an beiden Enden

Beispiel: Vom Laptop zur Wanddose oder vom Patchpanel zum Switch

## Verdrahtungsarten

### Straight-Through-Kabel

- Beide Enden haben die gleiche Farbbelegung
- TIA 568A & TIA 568B (Europa eher A, Amerika eher B) -> Andere Farbreihenfolge
- wird für Geräte verwendet unterschiedlichen OSI-Layers

| **Farbseite 1 (T568A)** | **Pins (T568A-T568A)** | **Farbseite 2 (T568A)** |
| ----------------------- | ---------------------- | ----------------------- |
| Weiß-Grün               | 1-1                    | Weiß-Grün               |
| Grün                    | 2-2                    | Grün                    |
| Weiß-Orange             | 3-3                    | Weiß-Orange             |
| Blau                    | 4-4                    | Blau                    |
| Weiß-Blau               | 5-5                    | Weiß-Blau               |
| Orange                  | 6-6                    | Orange                  |
| Weiß-Braun              | 7-7                    | Weiß-Braun              |
| Braun                   | 8-8                    | Braun                   |

| **Farbseite 1 (T568B)** | **Pins (T568B-T568B)** | **Farbseite 2 (T568B)** |
| ----------------------- | ---------------------- | ----------------------- |
| Weiß-Orange             | 1-1                    | Weiß-Orange             |
| Orange                  | 2-2                    | Orange                  |
| Weiß-Grün               | 3-3                    | Weiß-Grün               |
| Blau                    | 4-4                    | Blau                    |
| Weiß-Blau               | 5-5                    | Weiß-Blau               |
| Grün                    | 6-6                    | Grün                    |
| Weiß-Braun              | 7-7                    | Weiß-Braun              |
| Braun                   | 8-8                    | Braun                   |

### Crossover-Kabel

- Ein Ende hat die T568A-Belegung, das andere die T568B-Belegung.
- wird für Geräte verwendet desselben OSI-Modell-Layers

| **Farbe (T568A)** | **Pins (T568A-T568B)** | **Farbe (T568B)** |
| ----------------- | ---------------------- | ----------------- |
| Weiß-Grün         | 1-3                    | Weiß-Orange       |
| Grün              | 2-6                    | Orange            |
| Weiß-Orange       | 3-1                    | Weiß-Grün         |
| Blau              | 4-4                    | Blau              |
| Weiß-Blau         | 5-5                    | Weiß-Blau         |
| Orange            | 6-2                    | Grün              |
| Weiß-Braun        | 7-7                    | Weiß-Braun        |
| Braun             | 8-8                    | Braun             |

### OSI-Modell: Geräte Zusammenfassung

| **Physical** | **Data Link** | **Network** |
| ------------ | ------------- | ----------- |
| Hub          | Switch        | PC          |
|              |               | Server      |
|              |               | Router      |

## Auflegen von Kabeln

### Grobe Zusammenfassung

- gibt spezielle Werkzeuge (z. B. LSA Plus Auflege- bzw. Anlegewerkzeug)
- RJ45 Stecker crimpen mithilfe einer Crimpzange
- Kabel überprüfen mit einem Kabeltester (bei normalen Kabeln läuft das Aufblinken der Zahlennummerierung bei beiden Enden aufwärts parallel durch)

## Patchpanel

Ein Patchpanel ist eine Verteilerleiste für Netzwerkkabel, die für eine strukturierte Verkabelung sorgt.

### Patchpanel-Typen

- Punch-Down Patchpanel (Kabel werden mit LSA-Technik eingepresst, ideal für feste Installationen.)
- Keystone-Blank-Patchpanel (Leeres Panel für modulare Keystone-Module, flexibel anpassbar.)
- Coupler-Patchpanel (Verbindet Ports direkt über Kupplungen, ohne zusätzliche Verkabelung.)

### Arten von Patchpanels

- Ethernet-Patchpanel
- Glasfaser-Patchpanel
- Koaxial-Patchpanel
