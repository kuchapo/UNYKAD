# Allgemeine PowerShell-Befehle

## Datei suchen

```powershell
Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue | Where-Object { $_.Name -like '*berichtsheft*' }
```

## Windows-Ereignisprotokolle anzeigen

```powershell
get-winevent -logname system | select-object -first 5
```

## Datei mit Standard-Programm öffnen

=== "Alias"
    ```powershell
    ii "C:\Users\Xozah\Documents\UNYKAD\bitcoin.svg"
    ```

=== "Ausgeschrieben"
    ```powershell
    Invoke-Item "C:\Users\Xozah\Documents\UNYKAD\bitcoin.svg"
    ```

## Alle Cmdlets mit dem jeweiligen Parameter finden

```powershell
Get-command -CommandType Cmdlet -ParameterName Computername
```

## Alle Parameter eines Cmdlets finden

```powershell
Get-Help Join-Path -Parameter *
```

## Verzeichniserstellung

=== "New (PowerShell)"
    ```powershell
    new-item -itemtype directory <Name des Ordners>
    ```

=== "Old (CMD)"
    ```powershell
    mkdir <Name des Ordners>
    ```

## Textdatei erstellen (Beispiel):

1. `1..100 | foreach-object {add-content -path .\linenumbers.txt -value "This is line $_."}`
2. `get-content -path .\linenumbers.txt`
        -> sauber machen: clear-content …  
        -> ersten 5 anzeigen lassen: -totalcount 5  
        -> letzten 5 anzeigen lassen: -tail 5

## Nach etwas Sortieren

```powershell
# Mit dem Parameter sortiert es von Z-A, ohne von A-Z
ls | Sort-Object -descending
```

## Dateien überschreiben bzw. einer Datei hinzufügen:

- `>` und `>>`: Diese Zeichen sind einfach und schnell. `>` überschreibt eine Datei. `>>` fügt einer Datei was hinzu. (Redirection-Character)
- `Out-File`: Dieses Cmdlet macht im Grunde dasselbe, bietet aber mehr Kontrolle. Zum Beispiel kann man mit `Out-File` den Text in einer bestimmten Kodierung speichern oder bestimmen, ob man Zeilenumbrüche am Ende möchte.