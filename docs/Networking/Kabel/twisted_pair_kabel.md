# Twisted Pair Kabel

- Kupferkabel
- Buchse & Stecker: RJ45
- Acht Adern, die in vier Aderpaare (jeweils Hin- und Rückleiter) aufgeteilt sind
- rot, rot-weiß, grün, grün-weiß, blau, blau-weiß, braun, braun-weiß
- Bei jedem Adernpaar sind Hin- und Rückleiter miteinander verdrillt (twisted). Dadurch wirken die Magnetfelder, die der hinfließende Strom und der rückfließende Strom erzeugen, gegeneinander und heben sich im Idealfall auf.
- Parallele Leitungen wirken bei hohen Frequenzen wie Antennen, die sich gegenseitig beeinflussen. Deshalb sind bei besseren Kabeln die einzelnen Adernpaare gegeneinander abgeschirmt.
- 100Mbit nutzt nur 2 Adernpaare; 1000Mbit nutzt alle 4 Adernpaare
- CAT7 ist mit RJ45 nicht möglich λ=C/f

![Twisted Pair Kabel](./assets/twisted_pair_cable.drawio.svg)

## Bezeichnung

Bezeichnung nach ISO/IEC 11801 (2002): **X/Y**TP

| X: Gesamtschirmung                                                                                                       | Y: Adernpaarschirmung                                           |
| ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| - U = ungeschirmt<br>- F = Folienschirm (foiled)<br>- S = Geflechtschirm (screened)<br>- SF = Geflecht- und Folienschirm | - U = ungeschirmt<br>- F = Folienschirm<br>- S = Geflechtschirm |

(alte Bezeichnung PiMF: Paar in Metallfolie)

## Kategorien

| Kategorie | max. Frequenz | Anwendung                                            |
| --------- | ------------- | ---------------------------------------------------- |
| Cat1      | 100 kHz       | nur als UTP erhältlich, Telefonkabel                 |
| Cat2      | 1,5 MHz       | nur als UTP erhältlich ISDN                          |
| Cat3      | 16 MHz        | nur als UTP erhältlich, Telefonkabel, ISDN, 10Base-T |
| Cat4      | 20 MHz        | in Deutschland kaum erhältlich                       |
| Cat5      | 100 MHz       | 100 Base-T                                           |
| Cat5e     | 100 MHz       | höhere Anforderungen bei NEXT und FEXT, 1000Base-T   |
| Cat6      | 250 MHz       | 5Gbase-T und 10Gbase-T < 55m                         |
| Cat6A     | 500 MHz       | 10Gbase-T                                            |
| Cat7      | 600 MHz       | S/FTP                                                |
| Cat7A     | 1000 MHz      | S/FTP                                                |
| Cat8      | 2000 MHz      | 25Gbase-T und 40Gbase-T, S/FTP                       |

Seit 2003 entspricht Cat5 Kabel der Spezifikation von Cat5e.

## Crosstalk

- bezeichnet ein unerwünschtes elektrisches Signal, das von einem Adernpaar auf ein anderes überspringt
- Führt zu Störungen und Signalverlust

### NEXT (Near-End Crosstalk)

- Übersprechen am Anfang des Kabels (da, wo gesendet wird)
- Das gestörte Signal tritt nahe beim Sender auf

### FEXT (Far-End Crosstalk)

- Übersprechen am entfernten Ende des Kabels dort wo das Signal empfangen wird (am Anschluss nicht im Gerät)
