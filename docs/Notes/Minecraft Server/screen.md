# Screen

Screen ist eine einfache Lösung zum Server-Start im Hintergrund.

- Eine screen-Sitzung existiert nur so lange, wie der Prozess darin läuft.
- Sobald der Prozess endet (z. B. du tippst stop im Server oder exit in der Shell), wird die Sitzung automatisch beendet und gelöscht.

## Installation (falls noch nicht vorhanden)

```cli
sudo apt install screen
```

## Neue Sitzung erstellen

```cli
screen -S <Name der Sitzung>
```

## Mit vorhandenen Sitzung wiederverbinden

```cli
screen -r <Name der Sitzung>
```

## Sitzung schließen

```cli
stop
```

## Alle Sitzungen auflisten

```cli
screen -ls
```