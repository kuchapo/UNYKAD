# Backup

## Backup Arten

Vollbackup:

- Alle Daten werden gesichert
- Zeit- und Speicheraufwendig

Inkrementelles Backup:

- am Wochenende Vollbackup
- jeden Tag nur das, was sich seit dem letzten Backup verändert hat
- Also: Fr, Sa, So -> Backup & Rest der Woche nur die Änderung

Differenzielles Backup:

- am Wochenende Vollbackup
- jeden Tag nur das, was sich seit dem letzten Vollbackup geändert hat
- Also: Fr, Sa, So -> Vollbackup & Rest der Woche immer ein Backup Tag drauf, also am Montag sichert man sich nur Montag und am Dienstag Montag und Dienstag usw.

## 3-2-1 Regel

- 3 Backups auf 2 verschiedene Medien, 1 davon extern gelagert

## Großvater-Vater-Sohn-Regel

- Generationenprinzip
- 5 Tagesbänder (Mo, Di, Mi, Do, Fr)
- 4 Wochenbänder: am Wochenende werden die Tagesbänder auf ein Wochenband überspielt
- 12 Monatsbänder: am Monatsende werden die Wochenbänder auf einem Monatsband überspielt
- X Jahresbänder… mit 21 Bändern kann ein ganzes Jahr gesichert werden

## RAID ersetzt kein Backup

Wenn man RAID 1 hat, wo man alles doppelt hat, ersetzt es trotzdem kein Backup, weil wenn man sich ein Virus einfängt, kann es sein kann, dass trotzdem alles weg ist.
