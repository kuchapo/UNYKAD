# SQL Grundlagen

## Groß- und Kleinschreibung

`case-sensitive`: Unter **Windows** macht Groß- und Kleinschreibung bei Tabellen- und Datenbanknamen standartmäßig keinen Unterschied (also: "shop" = "Shop").  
`case-insensitive`: Unter **Linux** macht Groß- und Kleinschreibung bei Tabellen- und Datenbanknamen standartmäßig einen Unterschied (also: "shop" != "Shop"). Bei **Attributen** ist das unabhängig vom Betriebssystem genauso.

MySQL/MariaDB haben eine Server-Variable `lower_case_table_names`, die das Verhalten beeinflusst:

- `0`: Case-sensitive (Linux-Default)
- `1`: Alle Namen werden in Kleinbuchstaben gespeichert, Vergleiche sind case-insensitive (Windows-Default)
- `2`: Namen bleiben wie erstellt, Vergleiche aber case-insensitive (macOS-Default)

!!! info
	Einen anderen Wert als den Default-Wert zu wählen ist nicht empfohlen.

## Datenbänke

### Datenbank anlegen & nutzen

Man kann auf zwei Wege eine neue Datenbank erstellen. Beim ersten kommt eine Fehlermeldung, falls die Datenbank bereits existiert. Beim zweiten kommt keine.

==="mit Fehlermeldung"
	```sql
	CREATE DATABASE shop;
	USE shop;
	```

==="ohne Fehlermeldung"
	```sql
	CREATE DATABASE IF NOT EXISTS shop;
	USE shop;
	```
	
### Alle Datenbänke auflisten

```sql
SHOW DATABASES;
```

## Schemas

|                           | SQL-Standard                                                                                                                                                                                                                   | MySQL/MariaDB                                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Schemas (=Untereinheiten) | Eine Datenbank hat mehrere Schemas. Ein Schema kann mehrere Tabellen beinhalten.                                                                                                                                               | „Datenbank“ und „Schema“ sind Synonyme. `CREATE DATABASE test;` und `CREATE SCHEMA test;` machen exakt dasselbe. |
| Praxis-Beispiel           | Mit `SELECT * FROM shop.products;` kann man auf die Tabelle „products“ über das Schema „shop“ zugreifen. Zugriff auf andere Datenbanken mit zum Beispiel `SELECT * FROM database1.shop.products;` ist so direkt nicht möglich. | `SELECT * FROM shop.products;` greift über die Datenbank „shop“ auf die Tabelle „products“ zu.                   |

### Neues Schema erstellen

```sql
CREATE SCHEMA test;
```

### SQL-Standard

- Eine Datenbank kann mehrere Schemas (=Untereinheiten) enthalten, in welchen dann die Tabellen drin sind.
- Mit `SELECT * FROM shop.products;` kann man zum Beispiel auf die Tabelle „products“ über das Schema „shop“ zugreifen.
- Zugriff auf andere Datenbanken mit zum Beispiel `SELECT * FROM database1.shop.products;` ist so direkt nicht möglich.

### MySQL/MariaDB

- „Datenbank“ und „Schema“ sind Synonyme.
- `CREATE DATABASE test;` und `CREATE SCHEMA test;` machen exakt dasselbe.
- `SELECT * FROM shop.products;` greift über die Datenbank „shop“ auf die Tabelle „products“ zu.

## Entitäten und Attribute

Tabellen bzw. Entitäten beinhalten unterschiedliche **Datentypen**. In der Praxis hat jede Tabelle einen **Primary Key**, der jeden Datensatz eindeutig identifiziert. Wenn sich eine Tabelle auf eine andere bezieht, geschieht dies über einen **Foreign Key**, der auf den Primary Key einer anderen Tabelle verweist. Die Spalten der Tabellen bezeichnet man auch als Attribute. Jedes Attribut hat seinen eigenen **Datentyp**.

## Aggregatfunktionen

Die wichtigsten Aggregatfunktionen:

- `COUNT()`: Zählt die Anzahl der Zeilen in einer Tabelle oder Anzahl der nicht-NULL-Werte in einer Spalte. 
- `SUM()`: Berechnet die Summe aller numerischen Werte in einer Spalte. 
- `AVG()`: Ermittelt den Durchschnittswert (Mittelwert) einer Gruppe von Werten. 
- `MIN()`: Gibt den niedrigsten Wert in einer Gruppe von Werten zurück. 
- `MAX()`: Gibt den höchsten Wert in einer Gruppe von Werten zurück.
