# Repository verwalten

Projekt klonen und in den Hauptzweig wechseln:

```sh
git clone <PutYourProjectHere>
```

Python installieren (falls nicht vorhanden):

=== "Windows"
    ```powershell
    winget install Python.Python.3.11
    ```

=== "Arch Linux"
    ```bash
    pacman -S python
    ```

Virtuelle Umgebung erstellen und aktivieren:

```sh
python -m venv venv
```

=== "Windows"
    ```powershell
    ./venv/Scripts/activate
    ```

=== "Arch Linux"
    ```bash
    source ./venv/bin/activate
    ```

Abhängigkeiten installieren (falls erforderlich):

```sh
pip install -r requirements.txt
```

Holt Änderungen und integriert sie in die aktuelle lokale Branch:

```sh
git pull
```

Änderungen durchführen und in Git hinzufügen:

```sh
git add .
```

```sh
git commit -m "<EnterHereWhatYouHaveChanged>"
```

MkDocs-Projekt bauen:

```sh
mkdocs build
```

Deployment auf GitHub Pages:

```sh
mkdocs gh-deploy
```

Änderungen ins Repository pushen:

```sh
git push
```
