# Loopback

## Loopback-Adresse

gesamtes IPv4-Loopback-Netz ist 127.0.0.0/8

## Loopback Interface

Loopback-Interface (z. B. Loopback0) != Loopback-Adresse (z. B. 127.0.0.1)

Das Loopback-Interface auf einem Router ist immer aktiv, es sei denn es wird explizit ausgeschaltet. Dieses Interface sollte somit immer pingbar sein.

Wenn ein Router ein Loopback-Interface mit z. B. `192.168.1.1/32` hat, und gleichzeitig eine statische Route für `192.168.1.0/24` vorliegt, dann wird ein Paket an `192.168.1.1` **vom Router verarbeitet** und nicht vom /24-er Netz , weil die `/32`-Route **präziser** (spezifischer) ist als `/24`.

Loopback Interface erstellen:

```cli
int loopback <Number, mostly 0>
ip address <IP-Address> <Subnetmask>
no shut
```

Wenn die Subnetzmaske des Loopback-Interfaces länger ist als die der statischen Route und beide Routen auf das gleiche Zielnetz verweisen, dann wird das Paket **nicht über das Netzwerk weitergeleitet**, sondern **lokal verarbeitet**.