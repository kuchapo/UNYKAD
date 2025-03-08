# Repo import
## Windows
### Projekt klonen und in den Hauptzweig wechseln:
git clone <>
### Python installieren (falls nicht vorhanden):
winget install Python.Python.3.11
### Abhängigkeiten installieren (falls erforderlich):
pip install -r requirements.txt
### Virtuelle Umgebung erstellen und aktivieren:
python -m venv venv
./venv/Scripts/activate
### Änderungen durchführen und in Git hinzufügen:
git add .
git commit -m "EnterHereWhatYouHaveChanged"
### MkDocs-Projekt bauen:
mkdocs build
### Deployment auf GitHub Pages:
mkdocs gh-deploy
### Änderungen ins Repository pushen:
git push