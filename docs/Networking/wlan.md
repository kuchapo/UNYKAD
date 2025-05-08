# WLAN

## WLAN Standards

| Standard   | seit | Frequenz (GHz)   | Streams S/E  | Max. Rate / Stream  | Summe        | Kanalbreite (MHz) |
|------------|------|------------------|--------------|---------------------|--------------|-------------------|
| 802.11a    | 1999 | 5                | 1            | 54 Mbit             | 54 Mbit      | 20                |
| 802.11b    | 1999 | 2,4              | 1            | 11 Mbit             | 11 Mbit      | 20                |
| 802.11g    | 2003 | 2,4              | 1            | 54 Mbit             | 54 Mbit      | 20                |
| 802.11n    | 2009 | 2,4 + 5          | 4 x 4        | 4 x 150 Mbit        | 600 Mbit     | 40                |
| 802.11ac   | 2013 | 5 + 2,4*         | 8 x 8        | 8 x 867 Mbit        | 6,936 Gbit   | 80                |
| 802.11ad   | 2012 | 60               | 1            | 6,76 Gbit           | 6,76 Gbit    | 2160              |
| 802.11ay   | 2020 | 60               | 4 x 4        | 4 x 44 Gbit         | 176 Gbit     | 8640              |
| 802.11ax   | 2020 | 2,4 + 5          | 8 x 8        | 8 x 1,2 Gbit        | 9,6 Gbit     | 160               |
| 802.11be   | 2024 | 2,4 + 5 + 6      | 16 x 16      | 16 x 2,9 Gbit       | 46,4 Gbit    | 320               |

*802.11ac ist nur für 5 GHz spezifiziert aber ac Geräte können auch 2,4 Ghz nach Standard n nutzen  

**WIFI4**: 802.11n  
**WIFI5**: 802.11ac  
**WIFI6**: 802.11ax  
**WIFI6e**: nutzt zusätzlich das frei gewordene 6GHz Frequenzband  
**WIFI7**: 802.11be

---

## Frequenzen

### ISM Band (Industrial Scientific, Medical) – Lizenzfrei

Alle von WLAN genutzten Frequenzen sind lizenzfrei

### 2,4 GHz

* Frequenzbereich: 2399,5 – 2484,5 MHz
* 13 Kanäle bei 20 MHz Kanalbreite, 3 überlappungsfreie: **1, 6, 11**
* Bei 40 MHz nur noch 2 nahezu überlappungsfreie Kanäle: **3 und 11**
* In Deutschland: Sendeleistung bis **100 mW** erlaubt

**Vorteile:**

* überwindet abschirmende Materialien verlustärmer und hat so bei gleicher Sendeleistung eine höhere Reichweite als die anderen Frequenzen
* geringe Gerätekosten, da große Verbreitung und keine aufwändigen Spektrum-Management-Funktionen wie TPC oder DFS nötig

**Nachteile:**

* Frequenzband muss mit anderen Geräten beziehungsweise Funktechniken geteilt werden (Bluetooth, Mikrowellenherde, Babyphones, Schnurlostelefone, ...), dadurch Störungen und Interferenzen
* Störung durch andere WLANs wahrscheinlich, da nur wenige überlappungsfreie Kanäle zur Verfügung stehen

### 5 GHz

* 8 Kanäle im unteren Frequenzbereich (Bandbreite 20MHz)
* 36,40,44,48,52,56,60,64 (5,15 – 5.35 GHz)
* 11 Kanäle im oberen Frequenzbereich (Bandbreite 20MHz)
* 100,104,108,112,116,120,124,128,132,136,140 (5,47 – 5,725GHz)
* Die Frequenzen im oberen Frequenzbereich werden auch für Radarsysteme (Schifffahrt, Flugverkehr, Militär, Regenradar,...) genutzt. Daher besteht für diese in Deutschland  DFS Pflicht. Viele Router nutzen deshalb nur die Kanäle 36 – 48 im unteren Frequenzbereich.

    !!! info
        DFS (Dynamic Frequency Selection): Es wird selbstständig eine freie Frequenz gewählt, z. B. um das Stören von Radaranlagen zu vermeiden (z.B. Wetterradar).
        Bei Betrieb in Deutschland muss DFS auf den Kanälen 52 - 64 (5,25 - 5,35 GHz) und 100 - 140 (5,47 - 5,725 GHz) benutzt werden.

    Stärkere Regulierungen in Europa als auf den anderen Frequenzbändern: auf den meisten Kanälen DFS nötig, auf einigen Kanälen kein Betrieb im Freien erlaubt, falls kein TPC benutzt wird, muss die Sendeleistung reduziert werden.

    !!! info
        TPC (Transmit Power Control) reduziert ähnlich wie bei Mobiltelefonen die Sendeleistung abhängig von der Notwendigkeit (guter Kontakt zwischen den Geräten = geringere Sendeleistung).

**Vorteile:**

* deutlich höhere Übertragungsrate als bei 2,4 GHz möglich
* weniger genutztes Frequenzband, dadurch häufig störungsärmerer Betrieb möglich
* in Deutschland 19 nicht überlappende Kanäle
* höhere Reichweite, da mit 802.11h bis zu 1000 mW Sendeleistung möglich – das überkompensiert die größere Dämpfung der höheren Frequenzen

    !!! info
        Die Erweiterung 802.11h bezieht sich auf den Frequenzbereich von 5 GHz und besteht aus DFS und TPC. Wenn TPC implementiert ist, darf ein Gerät in Deutschland mit bis zu 1000mW Leistung senden.

**Nachteile:**

* DFS und TPC kosten Geld
* Ad-hoc-Modus wird von den meisten Geräten nicht unterstützt (weniger schlimm)
* Signal kommt schlechter durch Wände als 2,4 GHz

### 6 GHz

* frei gewordenes, lizenzfreies Frequenzband von 5,9 – 7,1 GHz
* kann von WIFI6E verwendet werden
* 14 Kanäle mit 80MHz Bandbreite oder 7 Kanäle mit 160MHz Bandbreite (überschneidungsfrei)
* Mit der Freigabe von Wi-Fi 6E und seinem erweiterten Frequenzbereich sowie neuen Geräten ist in Deutschland und Europa ab dem zweiten Halbjahr 2021 zu rechnen.
* WIFI6E benötigt neue Hardware, Erweiterung von WIFI6 auf WIFI6E per Firmware Upgrade ist nicht möglich.

### 60 GHz

* von 57 – 66 GHz, 4 Kanäle mit einer Bandbreite von 1760 Mhz, Kanalraster 2160MHz
* extrem hohe Datenrate
* sehr geringe Reichweite, Sender und Empfänger müssen im gleichen Raum sein, das Signal kommt durch keine Wand. Die Hersteller geben 10m Reichweite an, das ist optimistisch.
* Die 60GHz Standards sind nicht für flächendeckende Netze mit vielen Usern sondern für kurze drahtlose Punkt zu Punkt Verbindungen mit hohem Datenaufkommen gedacht. Beispiel 4K bzw. 8K Viedosignale im Home Entertainement Bereich.

---

## Parallele Datenströme

### MIMO – Multiple Input and Multiple Output

* ab Standard n
* Erhöhung der Übertragungsrate durch das gleichzeitige Senden mehrerer Signale zwischen zwei WLAN-fähigen Geräten über mehrere Antennen
* Der Standard n erlaubt 4x4 MIMO, dabei steht die erste Zahl für die maximale Anzahl von sendenden und die zweite für die empfangenden Antennen.
* Bei ac und ax sind sogar 8 Antennen und damit MIMO 8x8 möglich.
* Der 60GHz Standard ad kennt nur einen Datenstrom und damit kein MIMO, die Erweiterung ay kann 4 Datenströme zu einem 4x4 MIMO bündeln

### MU-MIMO – Multi-User MIMO

* Multiple User MIMO, funktioniert in den Standards ac ax und ay
* Damit können einzelne Clients ihre „eigenen“ Datenströme (Frequenzen) erhalten und damit gleichzeitig statt nacheinander mit dem Access Point kommunizieren.
* Theoretisch bis zu 8 Clients.

---

## WLAN Topologien

### Ad-hoc-Netz

**Independent Basic Service Set**  
Zwei oder mehrere Clients werden ohne einen Access Point direkt miteinander verbunden  
Nicht zu empfehlen – Bluetooth ist hier meist besser geeignet

### Infrastruktur-Netz

**Basic Service Set**  
Ein Access Point steuert die Kommunikation im WLAN und übernimmt die Verbindung zu kabelgebundenen Netzen (inkl. Internet)  
Das Netz bekommt einen Namen: die **SSID** (Service Set Identifier, bis 32 Bytes)

---

## Erweiterung des WLAN-Bereichs

### Repeater – WLAN-Controller – Mesh

#### Arten von Repeatern

**Repeater per Funk**  
    - Bei nur einer Antenne halbiert sich der Durchsatz  
    - Mit mehreren Antennen: Empfang z. B. auf 2,4 GHz, Weitergabe auf 5 GHz

**Repeater per Powerline (DLAN)**  
    - Kein Geschwindigkeitsverlust (sofern Powerline stabil funktioniert)

**Repeater per LAN-Kabel**  
    - Optimale Lösung – keine Verluste  
    - Praktisch nicht immer möglich

### Mehrere Access Points mit gleicher SSID

* Alle APs senden die **gleiche SSID** → wird zur **ESSID**  
* Unterschiedliche Kanäle vermeiden Störungen  
* Client wechselt bei Bewegung nur den Kanal (Roaming), nicht die SSID

#### WLAN-Controller

* Zentrale Steuerung mehrerer Access Points  
* Ein eigenes Gerät (Appliance)  
* Leichte Einrichtung und Überwachung (Clients, Traffic, etc.)  
* Steuerung des APs getrennt vom Nutzerdatenverkehr

#### Mesh

* Nur eine SSID  
* APs kommunizieren drahtlos über eigene Kanäle  
* Selbstorganisierend – kein WLAN-Controller nötig  
* Immer mindestens zwei Wege

#### Mehrzonen-WLAN

* Eigene WLANs für Gäste, IoT-Geräte etc.  
* APs müssen Multi-SSID und VLAN-Tagging unterstützen

#### Roaming

* Übergabe eines WLAN-Clients von einem AP an einen anderen beim Wechsel der Funkzelle

---

## Sicherheit

### SSID-Broadcast unterdrücken

* SSID wird normalerweise über **Beacons** ausgesendet  
* WLANs ohne sichtbare SSID erscheinen nicht im Menü  
* Sniffer wie **Wireshark** oder **Kismet** können SSID trotzdem auslesen

### MAC-Filter

* Liste erlaubter MAC-Adressen im Access Point  
* MAC-Adressen im 802.11-Frame sind unverschlüsselt → auslesbar  
* Spoofing mit bekannten MAC-Adressen ist möglich

### Verschlüsselungsarten

#### WEP

* **Veraltet** – innerhalb von Minuten knackbar  
* Nicht mehr sicher

#### WPA

* Schlüsselwechsel alle 2 Minuten  
* Ebenfalls nicht mehr sicher – kein Schutz bei heutigen Angriffsmethoden

#### WPA2

* Nutzt **AES-Verschlüsselung**  
* Sicher bei deaktiviertem **WPS**  
* Schwache Passwörter sind anfällig für Brute-Force-Angriffe

#### WPA3

* Verbesserte Sicherheit  
* Keine Wörterbuch-Angriffe mehr  
* Ersetzt WPS durch **Easy Connect** (z. B. QR-Code)  
* Öffentliche WLANs → jeder Client erhält eigenen Schlüssel

---

## Anmeldung

### Preshared Key (WPA2 Personal)

* Passwort auf jedem Gerät manuell eintragen  
* Einfach einzurichten  
* Admin kennt nur das Gerät, nicht den Benutzer

### RADIUS (WPA2 Enterprise)

* RADIUS (=Remote Authentication Dial In User Service)
* Anmeldung mit Benutzername und Passwort über Anmeldeserver  
* Admin erkennt den konkreten Nutzer  
* Zentral verwaltete Benutzerrechte  
* Gastzugang über zeitlich begrenzte **Voucher** möglich
