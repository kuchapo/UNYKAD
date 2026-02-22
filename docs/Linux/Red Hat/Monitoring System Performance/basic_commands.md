# Basic Commands

```bash
free -h					# zeigt RAM-Benutzung; Swap ist virtueller RAM, welcher benutzt wird wenn physischer Ram (Mem) voll ist; wenn Swap benutzt wird, dann wird Zeit RAM upzugraden
df -h					# zeigt Speicherauslastung pro gemountetem Dateisystem an
df -ih					# zeigt Inodes an; Dateisysteme haben eine limitierte Anzahl an Inodes & wenn alle benutzt werden, dann kann man keine neuen Dateien mehr erstellen
top
	# zeigt z. B. Prozess-ID und RES (Resident Memory Usage, also RAM ohne Swap); mit Taste 'h' sieht man Hilfe und mit ESC geht man da wieder raus
	# Mit Pfeiltasten nach Oben und Unten kann man durch Prozesse scrollen
	# Taste 'u' kann man Prozesse nach Benutzern filtern; erneutes 'u' und Enter macht den Filter wieder weg
htop
	# interagiert im Gegensatz zu top mit der Maus
	# F7 und F8 setzen den Nice-Wert; je höher dieser ist, desto weniger wird der Prozess priorisiert
vmstat 2 5
	# vmstat zeigt Systemressourcen über einen selbst festgelegten Zeitraum
	# 2 Sekunden Wartezeit zwischen den 5 Ausgabewerten
	# 'b'-Spalte steht für blockierte Prozesse; standardmäßig sollte dort nichts blockiert werden
	# 'swpd'-Spalte steht für den Swap-Speicher
	# 'si'-Spalte (swap-in): Menge an Speicher, die pro Sekunde aus dem Swap (Disk) zurück in den RAM geladen wird
	# 'so'-Spalte (swap-out): Menge an Speicher, die pro Sekunde aus dem RAM in den Swap (Disk) geschrieben wird
sysstat
	# ähnlich wie vmstat, nur kann es auch Aufzeichnen über einen Zeitraum hinweg; muss man erst mit dnf installieren
```