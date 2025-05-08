# Routing

- Dient dazu, den besten Weg von einem Netz in ein anderes Netz zu finden.
- Findet auf Layer 3 im OSI-Modell statt.
- Router kennt standartmäßig nur die Netze die direkt an ihn angeschlossen sind.
- Router entscheiden auf Grund ihrer Routingtabelle und der Destination IP wohin das Paket weitergeleitet wird.
- Wenn ein Router ein Paket bekommt, dessen Zielnetz nicht in seiner Routingtabelle steht, wirft er es weg.
- Routen zu entfernten (nicht direkt angeschlossenen) Netzen müssen in die Routingtabelle eingetragen werden (statisch von Hand oder dynamisch von Routingprotokollen wie z. B. RIP oder OSPF)

## Typ (Woher kennt der Router die Route?)

C bedeutet Connected (Router hat einen direkten Anschluss zu diesem Netz)  
S bedeutet Static Route, wurde vom Admin per Hand eingegeben (z. B. Default Route)  
R ist RIP (Routingprotokoll)  
O ist OSPF (Routingprotokoll)  
E ist EIGRP (Routingprotokoll)  
...

## Zielnetz mit entsprechender Subnetzmaske

- Netz! Nicht einzelne Hosts!

## Ausgangsport

Der Ausgangsport kann, muss allerdings nicht in der Routingtabelle stehen.

## next Hop

- IP der nächsten Routerschnittstelle (anderer Router) auf dem Weg zum Ziel
- Bei direkten Routen kann man den next Hop auslassen, wenn der Ausgangsport eingetragen ist

## Metrik

Ganze Zahl >= 0, die angibt, wie gut die Route ist (kleinere Zahl -> besser)

## Beispiel einer Routingtabelle

| Typ | Zielnetz    | Subnetzmaske  | (Ausgangsport) | next Hop     | Metrik |
| --- | ----------- | ------------- | -------------- | ------------ | ------ |
| C   | 192.168.1.0 | 255.255.255.0 | fa0/0          | -            | 0      |
| S   | 192.168.3.0 | 255.255.255.0 | fa1/0          | 192.168.2.2  | 1      |
| S   | 0.0.0.0     | 0.0.0.0       | (fa1/1)        | (82.137.5.9) | 1      |

Letzte Route ist die Default Route. Dort steht normalerweise meistens nur der Ausgangsport oder nur der next Hop. Beides, Ausgangsport und next Hop in der Default Route gilt als eher ungewöhnlich.

Abhilfe default route: Alles, was nicht in der Routingtabelle steht, schicke an next hop,...  
Die Reihenfolge der Einträge ist egal.
