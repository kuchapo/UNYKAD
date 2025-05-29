# CSMA (Carrier Sense Multiple Access)

Carrier Sense = Das Gerät "hört" erst, ob das Medium frei ist.  
Multiple Access = Mehrere Geräte teilen sich das Medium (z. B. das Funknetz oder ein Kabel)

## CSMA/CD

- Carrier Sense Multiple Access with **Collision Detection**

Wenn zwei Geräte zur gleichen Zeit senden, erfolgt eine Kollision. Durch ein Amplituden-Ausschlag werden beide Geräte informiert, dass die Daten nicht angekommen sind. Aus diesem Grund senden die Geräte nach Zufallszeit erneut ihre Daten.

## CSMA/CA

- Carrier Sense Multiple Access with **Collision Avoidance**

Kollisionen werden hier nicht erkannt, sie werden vermieden, indem die Geräte sich über die benötigte Übertragungsdauer gegenseitig informieren. Andere Geräte wissen somit wie lange das Medium nicht erreichbar ist.
