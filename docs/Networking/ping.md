# Ping

## Warum ohne Subnetzmaske?

Allgemein gilt, dass man beim Pingen die Subnetzmaske nicht mit eingibt, weil:

- Gerät selbst weiß, ob die IP im gleichen Netz ist oder nicht
- entscheidet dann, ob es direkt pingt oder es über das Gateway geht

## Routing mit übergreifender Netzmaske

### Beispiel zur Routenabdeckung

![ping](assets\routing_troubleshooting.drawio.svg)

**Situation**: Wenn beim Router A der statische Routen-Eintrag `ip route 192.168.10.0 255.255.255.0 172.16.0.2` durch `ip route 192.168.0.0 255.255.0.0 172.16.0.2` ersetzt wird, dann wird `192.168.10.8` von PC A aus trotzdem pingbar bleiben.

**Grund**: `192.168.178.1` passt in das Netz `192.168.0.0/16`