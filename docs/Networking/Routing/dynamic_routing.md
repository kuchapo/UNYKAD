# Dynamisches Routing

## Kurz zu Hops

Die Hops bei einem Router bezeichnen die Anzahl an ''Sprüngen'' von Router zu Router um ans Ziel zu kommen. Hops werden erst nach dem Default Gateway gezählt.

## Routing Protocol Kategorien

### Interior Gateway Protocols (IGP)

IGPs werden verwendet, um **Routen innerhalb eines einzelnen autonomen Systems (AS)** zu bestimmen – also z. B. im internen Netzwerk einer Firma. Beispiele sind **OSPF**, **EIGRP** und **RIP**. Sie sind für **interne Kommunikation** zwischen Routern gedacht.

| Distance Vector Protocols                          | Link State Protocols                              |
| -------------------------------------------------- | ------------------------------------------------- |
| Routing Information Protocol (RIP)                 | Open Shortest Path First (OSPF)                   |
| Interior Gateway Routing Protocol (IGRP)           | Intermediate System - Intermediate System (IS-IS) |
| Enhanced Interior Gateway Routing Protocol (EIGRP) |                                                   |

#### Distance Vector

- periodischer Austausch von Routingtabellen
- benutzen den Bellman-Ford oder Diffusion Update Algorithmus (DUAL)

#### Link State

- tauschen Link Inforamtionen mit dem gesamten Netzwerk aus
- benutzt Shortest Path First (SPF) als Algorithmus

### Exterior Gateway Protocols (EGP)

EGPs werden verwendet, um **Routen zwischen verschiedenen autonomen Systemen (AS)** zu steuern – z. B. zwischen Internetanbietern. Das bekannteste (und praktisch einzige genutzte) EGP ist **BGP**. Sie regeln also die **Verbindung zwischen Netzwerken** im Internet.

## Administrative Distance

Man kann in einem Netzwerk mehrere Protokolle am laufen haben. Der Router fügt die Route in seine Routing-Tabelle hinzu, welche die geringste AD aufweist.

-> Router wählt, welche Route zum Zielnetz zur Routing Tabelle hinzugefügt wird.

## Metrik

Bei der Metrik wird innerhalb eines Protokolles selbst entschieden, welche Route gewählt werden sollte. Wenn es also zwei OSPF-Routen gibt zum selben Zielnetz, dann wird anhand Bandbreite ausgewertet wie weit das Zielnetz entfernt ist.

| Protocol           | Metric            |
| ------------------ | ----------------- |
| Directly Connected | --                |
| Static Route       | --                |
| EIGRP              | Bandwidth + Delay |
| OSPF               | Bandwidth         |
| IS-IS              | Varies            |
| RIP                | Hop Count         |

## Begriffe

### Autonomous System (AS)

Ein AS bezeichnet die Netzwerkinfrastruktur innerhalb einer Organisation. Oder in schlau: Ein AS ist ein logisch zusammenhängendes Netzwerk, das unter einer einheitlichen Routing-Verwaltung steht.
