# AD: Befehle & Skripte (User)

## Etwas herausfinden

### Benutzer nach Erstellungsdatum sortieren

```powershell
Get-ADUser -Filter * -Property pwdLastSet, whencreated |
Select-Object DistinguishedName, Name, SamAccountname, pwdLastSet, whencreated |
sort-object whencreated |
Select-Object *,@{Name='PwdLastSet1'; Expression={[datetime]::FromFileTime($_.pwdLastSet)}} |
ft
```
### Benutzer nach letztem Anmeldedatum sortieren

=== "Möglichkeit 1"
	```powershell
	Get-ADUser -Filter * -Property lastLogon |
	Select-Object DistinguishedName, Name, SamAccountName, lastLogon |
	Sort-Object lastLogon |
	Select-Object *, @{Name='LastLogOnFormatiert'; Expression={[datetime]::FromFileTime($_.lastLogon)}} |
	ft
	```

=== "Möglichkeit 2"
	```powershell
	Get-ADUser -Filter * -Property lastLogon |
	Select-Object DistinguishedName, Name, SamAccountName, lastLogon |
	Sort-Object lastLogon |
	Select-Object DistinguishedName,Name,SamAccountName,@{Name='LastLogOn'; Expression={[datetime]::FromFileTime($_.lastLogon)}} |
	ft
	```

### AD-User, die "Enabled = True" sind

```powershell
get-aduser -Filter * | where-object enabled -eq true
```

### AD-User, die in der IT Abteilung sind

```powershell
# Pfad muss entsprechend angepasst werden
Get-adgroupmember -identity "CN=IT Abteilung,OU=Abteilungen,OU=Gruppen,DC=unykad,DC=com"
```

### AD-User, die seit einer Woche nicht mehr angemeldet waren

```powershell
$vorSiebenTagen = (Get-Date).AddDays(-7)
$benutzerNichtAngemeldet = Get-ADUser -Filter 'LastLogonDate -lt $vorSiebenTagen' -Properties LastLogonDate | Select-Object Name, SamAccountName, LastLogonDate | ft
$benutzerNichtAngemeldet
```
