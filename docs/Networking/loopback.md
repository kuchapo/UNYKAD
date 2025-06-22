# Loopback

## Loopback-Adresse

gesamtes IPv4-Loopback-Netz ist 127.0.0.0/8

## Loopback Interface

Loopback-Interface (z. B. Loopback0) != Loopback-Adresse (z. B. 127.0.0.1)

Das Loopback-Interface auf einem Router ist immer aktiv, es sei denn es wird explizit ausgeschaltet. Dieses Interface sollte somit immer pingbar sein.

Loopback Interface erstellen:

```cli
int loopback <Number, mostly 0>
ip address <IP-Address> <Subnetmask>
no shut
```

Wenn die Subnetzmaske des Loopback-Interfaces länger ist als die der statischen Route und beide Routen auf das gleiche Zielnetz verweisen, dann wird das Paket **nicht über das Netzwerk weitergeleitet**, sondern **lokal verarbeitet**.