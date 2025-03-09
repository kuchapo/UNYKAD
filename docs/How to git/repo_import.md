# Repo import
## Windows
Projekt klonen und in den Hauptzweig wechseln:  
```powershell
   git clone <>
```
Python installieren (falls nicht vorhanden):  
```powershell
winget install Python.Python.3.11
```
Abhängigkeiten installieren (falls erforderlich):  
```powershell
pip install -r requirements.txt
```
Virtuelle Umgebung erstellen und aktivieren:  
```powershell
python -m venv venv
```
```powershell
./venv/Scripts/activate
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
