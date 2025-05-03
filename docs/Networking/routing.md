# Routing

Dient dazu, den besten Weg von einem Netz in ein anderes Netz zu finden.

Layer 3 im OSI-Modell
  
Router entscheiden auf Grund ihrer Routingtabelle und der Destination IP.
Im IP Header des Paketes an welchem ihrer Routerports sie das Paket raus schicken.

## Typ (Woher kennt der Router die Route?)

C bedeutet Connected (Router hat einen Anschluss in diesem Netz)
S bedeutet Static Route, wurde vom Admin per Hand eingegeben
R ist RIP (Routingprotokoll)
O ist OSPF (Routingprotokoll)
...

## Zielnetz mit Subnetzmaske

Netz! Nicht einzelne Hosts!

## Ausgangsport

Der Ausgangsport kann, muss allerdings nicht in der Routingtabelle stehen.

## next Hop

IP der nächsten Routerschnittstelle (anderer Router) auf dem Weg zum Ziel

## Metrik

Ganze Zahl >= 0, die angibt, wie gut die Route ist (kleinere Zahl -> besser)

## Beispiel einer Routingtabelle

| Typ | Zielnetz    | Subnetzmaske  | (Ausgangsport) | next Hop     | Metrik |
| --- | ----------- | ------------- | -------------- | ------------ | ------ |
| C   | 192.168.1.0 | 255.255.255.0 | fa0/0          | -            | 0      |
| S   | 192.168.3.0 | 255.255.255.0 | fa1/0          | 192.168.2.2  | 1      |
| S   | 0.0.0.0     | 0.0.0.0       | (5fa1/1)       | (82.137.5.9) | 1      |

Letzte Route ist die Default Route.

Router kennt standartmäßig nur die Netze die direkt an ihn angeschlossen sind. Weiter entfernte Zielnetze müssen eingetragen werden (statisch  (Admin) oder dynamisch (Routingprotokoll)). Wenn ein Router ein Zielnetz nicht kennt, wirft er das Paket weg. Abhilfe default route: Alles, was nicht in der Routingtabelle steht, schicke an next hop,...
DIe Reihenfolge der EInträge ist egal.
