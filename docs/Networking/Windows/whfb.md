# Windows Hello for Business

## **Bereitstellungsmöglichkeiten**

Es gibt insgesamt **2 Optionen** für das Ausrollen von Windows Hello for Business:  

- **Tenant-wide** (für alle Benutzer)  
- **Für bestimmte Benutzergruppen**  

---

## **Windows Hello for Business aktivieren**

### **Unter Microsoft Intune admin center:**

1. **Devices**  
2. **Enrollment**  
3. **Windows Hello for Business**  

### **Alternative Methode:**

1. **Microsoft Intune admin center**  
2. **Devices**  
3. **Configuration**  
4. **Policies**  
5. **Create**  

---

## **Erstellen eines Profils**

1. **Platform:** Windows 10 and later  
2. **Profile Type:** Templates  
3. **Search Profile name:** Identity Protection

-> **Create**  

---

## **Identity Protection Einstellungen**

### **Next: WHFB auf 'Enable' und konfigurieren**

**Beispiele:**  

- **Minimum PIN length:** 8 Zeichen  
- **Enable PIN recovery:** ✅ Aktivieren  
Falls ein Benutzer die PIN vergisst, kann sie zurückgesetzt werden **ohne zusätzliche Faktoren** (z. B. Biometrie).  
Alle **Windows Hello for Business-Geräte** werden synchronisiert.  
Falls deaktiviert: Benutzer müssen PIN **komplett neu setzen**.  

### **Weitere Optionen:**

- **Use a Trusted Platform Module (TPM):** ✅ **Enable**  
Wenn aktiviert, wird die Verschlüsselung in **TPM** gespeichert (höhere Sicherheit).  
- **Allow biometric authentication:** ✅ **Enable**  
Ermöglicht **Fingerabdruck, Gesichtserkennung oder Retina-Scan**.  
- **Use enhanced anti-spoofing when biometric is enabled:** ✅ **Enable**  
Verhindert die Nutzung von **unechten biometrischen Daten** (z. B. Fotos).  
- **Certificates for on-premise resources:** ✅ **Enable**  
Falls **on-premises Zertifikate** für **Single Sign-On (SSO)** erforderlich sind.  
- **Use security keys for sign-in:** ✅ **Enable**  
Erlaubt die Nutzung von **FIDO2-Token** für zusätzliche Sicherheit.  

---

## **Gruppen zuweisen und abschließen**

- Nach der Konfiguration müssen die Richtlinien den jeweiligen **Gruppen zugewiesen** werden.  
- Anschließend wird Windows Hello for Business für die entsprechenden Benutzer **automatisch ausgerollt**.  
