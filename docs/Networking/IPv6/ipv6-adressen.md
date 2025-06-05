# IPv6-Adressen

## Vergabe der Adressen

Die Vergabe der IPv6-Adressen erfolgt durch die IANA (in Europa: RIPE) an die Provider.
Die ersten 32 Bits (/32) werden von der IANA an die Provider vergeben, sie sind festgelegt und bleiben unverändert.
Innerhalb des verbleibenden Adressraums können die Provider die Adressen flexibel an ihre Endkunden weitergeben, typischerweise in Blöcken wie /48, /56 oder /64. Als Standart wurde festgelegt, dass man bis zum 64. Bit Subnetze bilden kann.

Bsp.: Die deutsche Telekom vergibt an...  
Privatkunden /56 Blöcke -> 256 Netze  
Firmenkunden /48 Blöcke -> 65536 Netze

## Vergabe an Hosts

- manuelle Vergabe per Hand (feste IP)
- per DHCP (Dynamic Host Configuration Protocol)
- per SLAAC (Stateless Automatic Address Configuration)

### SLAAC

SLAAC ist ein Mechanismus, der es Geräten ermöglicht, ihre eigene weltweit gültige IPv6-Adresse basierend auf dem Netzwerkpräfix zu generieren.  

#### Präfix (Netzanteil)

**Router Advertisement (RA)**: Der Router gibt in gewissen Zeitabständen (einstellbar) das vom Provider erhaltene Präfix ins Netz bekannt.  
**Router Solicitation (RS)**: Der Host fragt den Router nach dem Präfix.

#### Interface Identifier (Hostanteil)

- **EUI64**: MAC-Adresse (Herstellerkennung + fffe + Kartennummer) und vorletztes Bit im ersten Byte wird gekippt  
- **Privacy Extensions**: Interface Identifier wird per Zufallsgenerator erzeugt

## Umwandlung

**Beispiel einer Umrechnung**:

- Ursprungs-IP: "2003:b10:0:0:1:1000:192.168.7.5"
- "192.168.7.5" wird zu "C0a8:705"
- Ergebnis: "2003:b10:0:0:1:1000:C0a8:705"

## Gemischte IPv6/IPv4-Adressen

### IPv4-Mapped IPv6-Adressen

**Darstellung Bsp.**: "*::ffff:166.207.88.26*"  
**Zweck**: Dient zur Darstellung von IPv4-Adressen in IPv6-Netzwerken. Wird häufig von Dual-Stack-Systemen genutzt, um IPv4-Clients mit IPv6-Servern zu verbinden.

### IPv4-Compatible IPv6-Adressen

**Darstellung Bsp.**: "*::166.207.88.26*"  
**Zweck**: Wurde früher verwendet, um IPv6-Adressen in IPv4-only-Netzwerken zu kapseln. Hier ist nur eine einseitige Kommunikation ohne zusätzlicher Mechanismen möglich (von IPv6 nach IPv4)

### 6to4-Adressen

**Darstellung Bsp.**: "*2002:166.207.88.26::/48*"  
**Zweck**: Ermöglicht die Kommunikation zwischen IPv6-Netzwerken über ein IPv4-Netzwerk (6to4-Tunneling). Hier ist keine direkte Kommunikation mit mit einer IPv4-Adresse möglich.

## Speziell reservierte IPv6-Adressen

- "**::**" sind nicht spezifierte Adressen, welche als Platzhalter dienen.  
- "**::1**" sind Loopback-Adressen, ähnlich wie in IPv4 mit "127.0.0.1", mit welchem man den localhost (=eigene Netzwerkschnittstelle) testen kann.

## Scopes (Gültigkeitsbereiche)

- **fe80::/10**  
sind Link-Local-Adressen, welche nur im eigenen LAN (=im lokalen Subnetz) gültig sind und nie durch den Router weitergeleitet werden.  
-> *1111 1110 10|00 0000*
- **fc00:: /7**  
sind Unique Local Adressen, welche auch zwischen verschiedenen Subnetzen innerhalb eines privaten Netzwerks kommunizieren können und nicht im Internet geroutet werden.  
-> *1111 110|0 0000 0000*
- **ff00::/8**  
sind Multicast-Adressen, welche an alle Geräte die Daten verschicken, welche sich für eine Multicast-Gruppe (über Multicast Listener Discovery (MLD)) angemeldet haben.  
-> *1111 1111|0000 0000*
- **2000::/3**  
sind globale Unicast-Adressen, welche weltweit einmalig im Internet aufzufinden sind.  
-> *001|0 0000 0000 0000*

### Sonderformen der Multicast-Adressen

"**ff01::1**" & "**ff02::1**" sind All-Nodes-Multicast-Adressen, welche die Daten an alle Nodes (Geräte) im Netzwerk schicken  
"**ff01::2**" & "**ff02::2**" bis "**ff05::2**" sind All-Routers-Multicast-Adressen, welche die Daten an alle Router schicken
