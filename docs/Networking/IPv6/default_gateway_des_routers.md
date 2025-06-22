# IPv6 Default Gateway des Routers

Der Router besitzt einmal eine globale und einmal eine lokale IPv6-Adresse. Die Endgeräte welche mit dem Router verbunden sind sehen nur die Link-Local-IPv6-Adresse.

Mit dem folgenden Befehl kann man auf dem Router eine Link-Local-Adresse konfigurieren (wie z. B. `fe80::1`)

```cli
ipv6 address <Link-Local-IPv6-Adresse> link-local
```

!!! info
	Die globale IPv6-Adresse wird trotzdem ebenso weiterhin pingbar sein

Zeigt konfigurierte ipv6 Interfaces an:

```cli
show ipv6 interface brief
```
