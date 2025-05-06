# Glasfaserkabel

Das Glasfaserkabel ist ein Lichtwellenleiter (LWL), welcher zur Datenübertragung Licht benutzt (Licht an: 1, Licht aus: 0).

## Totalreflexion

**Brechung** tritt auf, wenn Licht von einem Medium in ein anderes übergeht und dabei seine Richtung ändert. **Totalreflexion** passiert, wenn Licht von einem dichteren Medium (z. B. Wasser) in ein weniger dichtes Medium (z. B. Luft) trifft und der Einfallswinkel größer als der kritische Winkel ist, denn dann wird das Licht komplett reflektiert. Dieser Effekt wird in Glasfasern genutzt, um Licht verlustfrei durch Kurven zu übertragen.

## Multimode Kabel

- Faser 50µm oder 62,5µm dick (ungefähr wie ein menschliches Haar)
- können bis ca. 5 km lang sein
- Lichtquelle LED, streuendes Licht (viele Lichtstrahlen desselben Lichts), LED strahlt einen Frequenzbereich aus
- Dispersion: Die unterschiedlichen Lichtstrahlen gehen verschiedene Wege durch die Glasfaser, dadurch kommen sie nicht genau gleichzeitig am Ende der Glasfaser an (Einser gehen ineinander über und die Nullen verschwinden). Das Signal „verschwimmt“. Je länger das Kabel und je höher die Datenrate, desto stärker wirkt sich das aus.
- Für jedes Multimode-Kabel wird ein sogenanntes Bandbreiten-Längen-Produkt in MHz·km angegeben, das angibt, bis zu welcher Kombination aus Bandbreite und Kabellänge eine fehlerfreie Übertragung möglich ist. Formel: `Bandbreite x Länge = const in MHz·km`. Beispiel: Ein Multimode Kabel mit dem Bandbreiten-Länge-Produkt 1500MHz·km kann 1500MHz (entspricht 1500Mbit/s) über 1000m oder z.B. 750 MHz über 2000m sauber übertragen, wenn das Kabel nur 500m lang ist, kann es 3000MHz übertragen.
- Die Dämpfung bezeichnet ein Maß für die Abnahme der Helligkeit des Signals pro Kilometer im Kabel.

## Monomode (Singlemode) Kabel

- Faser nur 9µm dick
- können über 100km lang sein, dann muss aber der Lichtstrahl verstärkt werden
- Laserdiode als Lichtquelle, ein einziger Lichtstrahl, absolut monochromatisches Licht
- Unterschiedliche Glasorten drücken den Lichtstrahl immer in die Mitte des Kabels, hier gibt es keine Dispersion, nur Dämpfung.

## Multimode und Monomode Kabel

- Multiplex: es werden verschiedene Lichtfarben(Wellenlängen) gleichzeitig durch die Faser geschickt -> Erhöhung der Bandbreite des Kabels
- Dämpfung ist auch abhängig von der Lichtfarbe
- viele verschiedene Steckerformen
- eine Verbindung besteht immer aus einem Hin- und einem Rückleiter