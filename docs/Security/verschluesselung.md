# Verschlüsselung

## Symmetrisches Verschlüsseln

Es gibt einen geheimen Schlüssel, welcher die Nachrichten verschlüsseln und entschlüsseln kann.

**Vorteil**: Schnell und einfach
**Nachteil**: Schlüssel kann durch reines symmetrisches Verfahren nicht sicher beim Empfänger empfangen werden

### Beispiele

- `Cäsar-Chiffre`
	- Alle Buchstaben werden um desnselben festen Wert verschoben, zum Beispiel +3. So wird von einem A ein D.

        | Wort  | Beispiel | Verschlüsselung                                                            |
        | ----- | -------- | -------------------------------------------------------------------------- |
        | hallo | kdoor    | Schlüssel C (3 Buchstaben nach hinten vom Alphabet her)                    |
        | hallo | keopr    | Schlüssel CD (erster Buchstabe 3 nach hinten, zweiter 4 nach hinten, usw.) |

- `Vigenère-Verschlüsselung`
	- Anstatt einen **festen Schlüssel (+3)** wie bei Cäsar zu nehmen, benutzt man ein **Wort oder eine Zeichenfolge** als Schlüssel.
	- Jeder Buchstabe des Schlüssels bestimmt, um wie viele Stellen der Klartext verschoben wird.
	- Der Schlüssel wird dabei **wiederholt** über den Klartext gelegt.

- `DES`

- `AES (Advanced Encryption Standard)`
	- Ist der neue Standard, der alte war DES
	- benutzt Blockchiffre/Blockverschlüsselung, wo Klartext mit 16 Buchstaben in ein Block gepackt wird
	- Jeder Block hat seinen eigenen Schlüssel zur Verschlüsselung
	- Es werden variable voneinander unabhängige Block- und Schlüssellängen von 56-Bit (sehr unsicher), 128-Bit, 192-Bit und 256-Bit verwendet
	- Rundenanzahl ist abhängig von der Schlüssellänge
		- 128-Bit: 10 Runden
		- 192-Bit: 12 Runden
		- 256-Bit: 14 Runden
	- Verschlüsselungsprozess
		- Klartextblock wird mit dem RoundKey zusammengerechnet und es entsteht eine neue Zustandsmatrix (AddRoundKey)
		- Folgende Stationen durchgeht dann der Byte-Block 9 Mal hintereinander bei 128-Bit:
			- SubBytes:
				- Zustand wird mithilfe der S-BOX verschlüsselt
			- ShiftRows
				- 1 Byte-Rotation in der zweiten Zeile: 1 Byte von ganz links nach ganz rechts verschoben
				- 2 Byte-Rotation in der dritten Zeile: 2 Bytes von ganz links nach ganz rechts verschoben
				- 3 Byte-Rotation in der vierten Zeile: 3 Bytes von ganz links nach ganz rechts verschoben
			- MixColumns
				- komplexe Spalten-Multiplikation mit dem "Rijndael Galios Feld"
			- AddRoundKey
				- Erstellung des ersten RoundKeys
					- bei der letzten Block-Spalte vom Schlüssel wird der erste Byte von oben nach ganz unten verschoben
					- wie bei SubBytes wird diese Spalte mithilfe der S-BOX verschlüsselt
					- man addiert nun die erste Spalte des ursprünglichen Schlüssels mit der berechneten letzten Spalte sowie der ersten Spalte der statischen Rcon Tabelle zusammen -> Erste Spalte des RoundKeys
					- man addiert dann die zweite Spalte des ursprünglichen Schlüssels mit der ersten Spalte des RoundKeys -> Zweite Spalte des RoundKeys
					- man addiert die dritte Spalte des ursprünglichen Schlüssels mit der zweiten Spalte des RoundKeys -> Dritte Spalte des RoundKeys
					- dann addiert man die vom ursprünglichen Schlüssel vierte Spalte mit dem RoundKey der dritten Spalte -> vierte Spalte des RoundKeys
				- Erstellung der weiteren RoundKeys
					- funktioniert genau gleich wie beim Erstellen des ersten RoundKeys, nur mit dem Unterschied, dass immer beim weiteren Erstellen des RoundKey bei der Rcon-Tabelle dann die Spalte weiter rechts genommen wird (beim ersten RoundKey wird die erste Spalte der Rcon-Tabelle genommen, beim zweiten RoundKey die zweite Spalte, usw.)
		- Nach den 9 Runden durchgeht der Block nochmal eine Runde, nur diesmal ohne der Station MixColumns

## Asymmetrisches Verschlüsseln

Es gibt einen öffentlichen und einen privaten Schlüssel. Nachrichten werden immer mit dem öffentlichen Schlüssel des Empfängers verschlüsselt und können nur mit dessen privatem Schlüssel entschlüsselt werden. Der private Schlüssel bleibt dabei stets geheim und verlässt das Gerät nicht. Der private Schlüssel kann aus dem öffentlichen Schlüssel nicht berechnet werden.

**Vorteil**: Sicher
**Nachteil**: Langsam, etwa 10.000 mal langsamer als asymmetrische Verschlüsselung

### Beispiele

- `RSA (Rivest–Shamir–Adleman)`
	- Ein Computer kann mit Buchstaben nichts anfangen → darum werden sie in Zahlen codiert (z. B. mit ASCII oder Unicode). Bsp.: Aus A wird 65 und aus B die Zahl 66.
	- Die **Verschlüsselungsformel** lautet `c = m^e mod n`. Public Key = (n, e)
		- **c** = Ergebnis = verschlüsselte Zahl
		- **m** = Zahl der Nachricht (z. B. 65 für „A“)
		- **e** = öffentlicher Exponent (Teil des Public Key)
		- **n** = Modulus (Teil des Public Key)
	- Beispielrechnung zur Verschlüsselung:
		- Gegeben: m = 7, e = 3, n = 33
		- Einsetzen in Formel: `c = 7^3 mod 33` = `c = 343 mod 33`
		- 343 / 33 = 10 Rest 13
		- c = 13
	- Die **Entschlüsselungsformel** lautet `m = c^d mod n`. Private Key = (n, d)
		- **d** = privater Exponent
	- Beispielrechnung zur Entschlüsselung:
		- Gegeben: c = 13, d = 7, n = 33
		- Einsetzen in Formel: `m = 13^7 mod 33` = `m = 62748517 mod 33`
		- m = 7
	- Bei der Schlüsselgenerierung vom öffentlichen und privaten Schlüssel werden zunächst zwei große Primzahlen genommen, welche über 100 Stellen lang und geheim sind. Daraus wird dann der Modulus n berechnet (`n = p × q`), welcher später öffentlich wird. Dann wird eine Zahl e als öffentlicher Exponent gewählt (typisch: 65537). e muss dabei teilerfremd zu (p−1)(q−1) sein. Zum Schluss wird ein privater Exponent d berechnet, so dass folgendes gilt: `e × d ≡ 1 (mod (p − 1)(q − 1))`.
	- Sicherheit
		- Es ist extrem schwer, n wieder in p und q zu zerlegen (Primfaktorzerlegung).
		- Ohne p und q kann man d nicht berechnen.

- `Rabin`

- `Elgamal`

## Hybrides Verschlüsseln

Der öffentliche Schlüssel der Symmetrischen Verschlüsselung wird über Asymmetrische Verschlüsselung übermittelt. Danach wird nur noch über Symmetrische Verschlüsselung kommuniziert. Beispiele: `SSL` & `TSL`

Vorteil: Sicher und schnell
Nachteil: hat keinen

## Einwegverschlüsselung

Zeichenfolge wird verschlüsselt, aus den verschlüsselten Zeichen kann das Original nicht mehr hergestellt werden.

Zur Verschlüsselung von Passwörtern, Hashes
