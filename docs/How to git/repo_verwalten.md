# Repository verwalten

Projekt klonen und in den Hauptzweig wechseln:  
```powershell
git clone <>
```
Python installieren (falls nicht vorhanden):  
```powershell
winget install Python.Python.3.11
```
```info
Bei Linux muss dieser Befehl angepasst werden!"
```
Virtuelle Umgebung erstellen und aktivieren:  
```powershell
python -m venv venv
```
```powershell
./venv/Scripts/activate
```
```info
Bei Linux: "source ./venv/bin/activate"
```
Abhängigkeiten installieren (falls erforderlich):  
```powershell
pip install -r requirements.txt
```
Änderungen durchführen und in Git hinzufügen:  
```powershell
git add .
```
```powershell
git commit -m "EnterHereWhatYouHaveChanged"
```
MkDocs-Projekt bauen:  
```powershell
mkdocs build
```
Deployment auf GitHub Pages:  
```powershell
mkdocs gh-deploy
```
Änderungen ins Repository pushen:  
```powershell
git push
```
