# SQL-Datentypen (Dialekt-übergreifend)

Diese Datei listet die gängigen SQL-Datentypen **pro Datenbank-Dialekt** – gruppiert nach sinnvollen Kategorien. Kurze Erklärungen helfen bei der Auswahl des passenden Typs.

> ⚠️ Hinweise
> 
> - Nicht jeder Typ existiert in jedem Dialekt; einige sind **Alias**e oder **veraltet**.  
> - Manche Features erfordern **Erweiterungen** (z. B. PostGIS für Postgres).  
> - Versionen können Verhalten ändern (z. B. JSON-Unterstützung, Zeitstempel-Details).

---

## 🔢 Numerische Datentypen (Ganzzahlen & Genau/ungefähr)

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`TINYINT`|Sehr kleine Ganzzahl (i. d. R. −128…127 / `UNSIGNED` 0…255). Wird oft auch als Bool-Ersatz genutzt.|
    |`SMALLINT`|Kleine Ganzzahl (−32.768…32.767).|
    |`MEDIUMINT`|Mittlere Ganzzahl (−8.388.608…8.388.607).|
    |`INT` / `INTEGER`|Standard-Ganzzahl (±2.147.483.647).|
    |`BIGINT`|Sehr große Ganzzahl (±9.22e18).|
    |`DECIMAL(p,s)` / `NUMERIC(p,s)`|**Exakte** Dezimalzahl (Geld, Mengen). In MySQL max. ~`DECIMAL(65,30)`.|
    |`FLOAT`|Gleitkomma (≈ single precision, **ungefähr**).|
    |`DOUBLE` / `DOUBLE PRECISION`|Gleitkomma (≈ double precision, **ungefähr**).|
    |`BIT(m)`|Bit-Feld mit `m` Bits (0/1-Muster).|
    |`SERIAL`|Alias für `BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE`.|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`SMALLINT`|16‑Bit Ganzzahl.|
    |`INTEGER` / `INT`|32‑Bit Ganzzahl.|
    |`BIGINT`|64‑Bit Ganzzahl.|
    |`NUMERIC(p,s)` / `DECIMAL`|**Exakt**; ideal für Geld.|
    |`REAL`|32‑Bit Gleitkomma (**ungefähr**).|
    |`DOUBLE PRECISION`|64‑Bit Gleitkomma (**ungefähr**).|
    |`SMALLSERIAL` / `SERIAL` / `BIGSERIAL`|Autoinkrement-Pseudotypen (ganzzahlig).|
    |`MONEY`|Währungstyp (regional formatiert; Vorsicht bei Rechenlogik).|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`TINYINT`|0…255.|
    |`SMALLINT`|−32.768…32.767.|
    |`INT`|Standard 32‑Bit.|
    |`BIGINT`|64‑Bit.|
    |`DECIMAL(p,s)` / `NUMERIC(p,s)`|**Exakt**.|
    |`SMALLMONEY` / `MONEY`|Währungs-Typen (Rundung/Region beachten).|
    |`FLOAT(n)`|IEEE754, Präzision über `n` (1–53) gesteuert.|
    |`REAL`|`FLOAT(24)` Alias.|
    |`BIT`|0/1.|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |**Storage-Klassen**|`INTEGER`, `REAL`, `NUMERIC`, `TEXT`, `BLOB` (dynamische Typisierung).|
    |`INTEGER`|Ganzzahl beliebiger Größe (bis 8 Byte).|
    |`REAL`|8‑Byte Gleitkomma.|
    |`NUMERIC` / `DECIMAL(p,s)`|Numerische Affinität (Werte oft **exakt** gespeichert, je nach Eingabe).|
    |_(Bool/Geld)_|Es gibt keinen speziellen `BOOLEAN`/`MONEY`; üblich sind `INTEGER` (0/1) bzw. `NUMERIC`.|

==="Oracle Database"

    | Datentyp                             | Beschreibung                                              |
    | ------------------------------------ | --------------------------------------------------------- |
    | `NUMBER(p,s)`                        | **Exakt**; Standard-Zahlentyp (ohne `p,s` sehr flexibel). |
    | `FLOAT`                              | Dezimal浮punkt (subtype von `NUMBER`).                     |
    | `BINARY_FLOAT` / `BINARY_DOUBLE`     | IEEE754-Gleitkomma (ungefähr).                            |
    | `INTEGER`/`SMALLINT`/`DEC`/`DECIMAL` | Synonyme/Subtypen von `NUMBER`.                           |

---

## 📝 Zeichen- und Text-Datentypen

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`CHAR(n)`|Feste Länge.|
    |`VARCHAR(n)`|Variable Länge.|
    |`TINYTEXT` / `TEXT` / `MEDIUMTEXT` / `LONGTEXT`|Sehr lange Texte, gestaffelte Maximalgrößen.|
    |`NCHAR(n)` / `NVARCHAR(n)`|„National Character“ Varianten (Unicode).|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`CHAR(n)`|Feste Länge.|
    |`VARCHAR(n)`|Variable Länge.|
    |`TEXT`|Unbegrenzt (praktisch). Alle Strings sind UTF‑8 (je nach Cluster-Encoding).|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`CHAR(n)` / `VARCHAR(n)`|1‑Byte pro Zeichen (Codepage).|
    |`VARCHAR(MAX)`|Bis 2 GB Text.|
    |`NCHAR(n)` / `NVARCHAR(n)`|UTF‑16 (2‑Byte).|
    |`NVARCHAR(MAX)`|Bis 2 GB Unicode-Text.|
    |_(veraltet)_ `TEXT`/`NTEXT`|Alte Large-Text-Typen (nicht mehr verwenden).|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |`TEXT`|Beliebige Länge, UTF‑8/16. Längen werden nicht hart erzwungen.|
    |Synonyme|Namen wie `VARCHAR(255)` werden als `TEXT`-Affinity behandelt.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`CHAR(n)` / `VARCHAR2(n)`|Standard-Texttypen.|
    |`NCHAR(n)` / `NVARCHAR2(n)`|Unicode-Varianten.|
    |`CLOB` / `NCLOB`|Große Textobjekte.|
    |_(veraltet)_ `LONG`|Alter Langtexttyp (nicht mehr verwenden).|

---

## 📅 Datums- und Zeit-Datentypen

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`DATE`|Datum.|
    |`TIME[(fsp)]`|Uhrzeit (optional Präzision `fsp`).|
    |`DATETIME[(fsp)]`|Datum+Zeit **ohne** Zeitzone.|
    |`TIMESTAMP[(fsp)]`|Datum+Zeit, intern UTC, Konvertierung nach Session-TZ.|
    |`YEAR`|Jahr (z. B. `YEAR(4)`).|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`DATE`|Datum.|
    |`TIME [WITHOUT/WITH TIME ZONE]`|Uhrzeit (optional mit TZ).|
    |`TIMESTAMP [WITHOUT/WITH TIME ZONE]`|Zeitstempel (mit/ohne TZ).|
    |`INTERVAL`|Zeitspanne.|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`DATE`|Datum.|
    |`TIME(p)`|Uhrzeit mit Präzision.|
    |`DATETIME`|Datum+Zeit (Millisekunden, historisch).|
    |`DATETIME2(p)`|Modernere, präzisere Variante.|
    |`SMALLDATETIME`|Minuten-genau, kleiner Bereich.|
    |`DATETIMEOFFSET(p)`|Datum+Zeit **mit** Zeitzone.|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |_(kein eigener Typ)_|Speicherung als `TEXT` (ISO‑8601), `REAL` (Julian Day) oder `INTEGER` (Unix-Zeit). Funktionen kümmern sich um Umrechnung.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`DATE`|Datum **mit** Zeitanteil (Sekunden).|
    |`TIMESTAMP(n)`|Datum+Zeit mit Präzision.|
    |`TIMESTAMP WITH TIME ZONE` / `LOCAL TIME ZONE`|Zeitstempel mit TZ-Handhabung.|
    |`INTERVAL YEAR TO MONTH` / `INTERVAL DAY TO SECOND`|Zeitspannen.|

---

## ✅ Boolesche & Mengen-Typen

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`BOOLEAN` / `BOOL`|Alias für `TINYINT(1)` (0/1).|
    |`ENUM('A','B',...)`|Einer aus vordefinierten Werten.|
    |`SET('A','B',...)`|**Mehrere** vordefinierte Werte kombinierbar.|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`BOOLEAN`|`TRUE`/`FALSE`/`NULL`.|
    |`ENUM` (benutzerdefiniert)|`CREATE TYPE ... AS ENUM`.|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`BIT`|0/1/`NULL`. _(Kein `ENUM`; per `CHECK`-Constraint modellieren.)_|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |_(kein eigener Bool)_|Üblich: `INTEGER` 0/1. `CHECK (col IN (0,1))` empfehlenswert.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |_(Boolean in Tabellen)_|Traditionell kein nativer `BOOLEAN` in Tabellen; üblich: `NUMBER(1)`/`CHAR(1)` + `CHECK`. (Neuere Versionen können native JSON/weitere Features bieten.)|

---

## 💾 Binär- & Bit-String-Typen

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`BINARY(n)` / `VARBINARY(n)`|Feste/variable Länge Binärdaten.|
    |`TINYBLOB` / `BLOB` / `MEDIUMBLOB` / `LONGBLOB`|Große Binärobjekte.|
    |`BIT(m)`|Bit-Felder (auch für Flags).|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`BYTEA`|Binärdaten.|
    |`BIT(n)` / `BIT VARYING(n)`|Bit-Strings fester/variabler Länge.|

==="SQL Server"

    | Datentyp                   | Beschreibung                                           |
    | -------------------------- | ------------------------------------------------------ |
    | `BINARY(n)` / `VARBINARY(n | MAX)`                                                  |
    | _(veraltet)_ `IMAGE`       | Alter Large-Binary-Typ.                                |
    | `ROWVERSION` (`TIMESTAMP`) | Automatische Versionsnummer (8‑Byte, **kein** Datum!). |

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |`BLOB`|Beliebige Binärdaten.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`RAW(n)`|Binärdaten bis 2000 Bytes.|
    |`LONG RAW`|Veraltet.|
    |`BLOB`|Großes Binärobjekt.|
    |`BFILE`|Externe Binärdatei (nur Verweis, read‑only).|

---

## 🧩 JSON, XML & semistrukturiert

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |`JSON`|Nativer JSON-Typ mit Validierung & Funktionen.|
    |_(XML)_|Kein eigener XML-Typ; Speicherung als Text/Blob.|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`JSON`|Textbasiert, originalgetreu.|
    |`JSONB`|Binärrepräsentation (indizierbar, schneller für Abfragen).|
    |`XML`|XML-Datentyp + XPath/XQuery-Funktionen.|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`XML`|XML mit Schema-Unterstützung.|
    |_(JSON)_|Kein eigener Typ; Speicherung als `NVARCHAR` + `ISJSON()` und JSON‑Funktionen.|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |_(JSON)_|Kein eigener Typ; Speicherung als `TEXT` + JSON1‑Funktionen (`json_valid()` usw.).|
    |_(XML)_|Als `TEXT` speichern.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`XMLTYPE`|XML mit nativer Verarbeitung.|
    |_(JSON)_|Moderne Versionen bieten **nativen JSON**; traditionell `CLOB`/`BLOB` mit `IS JSON`‑Constraint.|

---

## 📐 Geodaten & Geometrie

==="MySQL / MariaDB (OpenGIS)"

    |Datentyp|Beschreibung|
    |---|---|
    |`GEOMETRY`|Basistyp.|
    |`POINT` / `LINESTRING` / `POLYGON`|Grundformen.|
    |`MULTIPOINT` / `MULTILINESTRING` / `MULTIPOLYGON` / `GEOMETRYCOLLECTION`|Sammlungen.|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |_(Kern)_ `POINT`, `LINE`, `LSEG`, `BOX`, `PATH`, `POLYGON`, `CIRCLE`|Geometrische Typen (rein geometrisch).|
    |_(Erweiterung: PostGIS)_ `GEOMETRY`, `GEOGRAPHY`, `RASTER`, …|Vollwertige GIS-Typen & Funktionen.|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`GEOMETRY`|Planare Geometrie.|
    |`GEOGRAPHY`|Geodätische Koordinaten (ellipsoidisch).|
    |`HIERARCHYID`|Baum-/Hierarchiepfade (kein Geo, aber „räumlich“ strukturiert).|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |_(SpatiaLite)_|Über Erweiterung: GIS‑Typen/Funktionen. Im Kern: Speicherung als `BLOB`/`TEXT`.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`SDO_GEOMETRY`|Oracle Spatial/Locator Geometrietyp.|

---

## 🌐 Netzwerk, UUID & Spezialtypen

==="MySQL / MariaDB"

    |Datentyp|Beschreibung|
    |---|---|
    |_(UUID)_|Kein eigener Typ; `CHAR(36)` (String) oder `BINARY(16)` (kompakt). Funktionen wie `UUID()`, `UUID_TO_BIN()`.|

==="PostgreSQL"

    |Datentyp|Beschreibung|
    |---|---|
    |`UUID`|Nativ; `gen_random_uuid()` (via `pgcrypto`).|
    |`INET` / `CIDR`|IP‑Adresse / Netzblock.|
    |`MACADDR` / `MACADDR8`|MAC‑Adressen.|
    |`TSVECTOR` / `TSQUERY`|Volltext-Indizierung/Anfrage.|
    |`HSTORE`|Key‑Value‑Store (Extension).|
    |`RANGE`‑Typen|`INT4RANGE`, `INT8RANGE`, `NUMRANGE`, `TSRANGE`, `TSTZRANGE`, `DATERANGE`.|
    |`ARRAY`|Arrays jeden Basistyps (`integer[]` etc.).|
    |`DOMAIN` / `COMPOSITE`|Eigene abgeleitete/zusammengesetzte Typen.|

==="SQL Server"

    |Datentyp|Beschreibung|
    |---|---|
    |`UNIQUEIDENTIFIER`|GUID/UUID.|
    |`SQL_VARIANT`|Hält Werte verschiedener Skalartypen.|
    |`ROWVERSION`|Zeilenversionierung (autom. 8‑Byte).|
    |`HIERARCHYID`|Hierarchiepfade.|
    |_(Netzwerk)_|Kein eigener `INET`/`CIDR`; per `VARCHAR` + Constraints.|

==="SQLite"

    |Datentyp|Beschreibung|
    |---|---|
    |_(UUID)_|Als `TEXT` (36) oder `BLOB` (16).|
    |_(Netzwerk)_|Als `TEXT` + `CHECK`.|

==="Oracle Database"

    |Datentyp|Beschreibung|
    |---|---|
    |`ROWID` / `UROWID`|Physische/Logische Zeilenadresse.|
    |_(UUID)_|Üblich: `RAW(16)`; `SYS_GUID()` erzeugt UUID‑Wert.|
    |`ANYTYPE` / `ANYDATA`|Metadaten-/generische Typcontainer.|
    |_(Sammlungen)_|`VARRAY`, `NESTED TABLE` (benutzerdefinierte Collection‑Typen).|

---

## 🧪 Mini‑Beispiele (CREATE TABLE)

```sql
-- MySQL/MariaDB
CREATE TABLE orders (
  id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  amount DECIMAL(12,2) NOT NULL,
  status ENUM('open','paid','canceled') NOT NULL,
  meta JSON,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- PostgreSQL
CREATE TABLE orders (
  id BIGSERIAL PRIMARY KEY,
  amount NUMERIC(12,2) NOT NULL,
  status order_status NOT NULL, -- vorher: CREATE TYPE order_status AS ENUM('open','paid','canceled');
  meta JSONB,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- SQL Server
CREATE TABLE orders (
  id BIGINT IDENTITY(1,1) PRIMARY KEY,
  amount DECIMAL(12,2) NOT NULL,
  status NVARCHAR(16) CHECK (status IN ('open','paid','canceled')) NOT NULL,
  meta NVARCHAR(MAX) NULL, -- JSON per CHECK(ISJSON(meta)) möglich
  created_at DATETIME2(3) DEFAULT SYSUTCDATETIME()
);

-- SQLite
CREATE TABLE orders (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  amount NUMERIC NOT NULL,
  status TEXT CHECK (status IN ('open','paid','canceled')) NOT NULL,
  meta TEXT, -- JSON1-Funktionen, optional CHECK (json_valid(meta))
  created_at TEXT DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now'))
);

-- Oracle
CREATE TABLE orders (
  id NUMBER GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
  amount NUMBER(12,2) NOT NULL,
  status VARCHAR2(16) CHECK (status IN ('open','paid','canceled')) NOT NULL,
  meta CLOB,
  created_at TIMESTAMP DEFAULT SYSTIMESTAMP
);
```

---

## ✅ Auswahlhilfe (Kurz)

- **Geld** → _exakt_: `DECIMAL/NUMERIC` (MySQL/PG/SQL Server), `NUMBER(p,s)` (Oracle)
    
- **Große Zähler** → `BIGINT` / `BIGSERIAL` / `IDENTITY BIGINT`
    
- **Freitext** → `TEXT`/`CLOB`/`NVARCHAR(MAX)`/`TEXT`
    
- **JSON** → `JSON` (MySQL), `JSONB` (PG), `NVARCHAR(MAX)` + `ISJSON()` (SQL Server), `TEXT` + JSON1 (SQLite), `JSON`/`CLOB` (Oracle, abhängig von Version)
    
- **Zeitzonen** → `TIMESTAMPTZ` (PG), `DATETIMEOFFSET` (SQL Server), `TIMESTAMP` (MySQL, aber TZ‑Konvertierung beachten)
    
- **Binär** → `BYTEA` (PG), `VARBINARY`/`BLOB` (andere)
    
- **Geo** → PostGIS (PG), `GEOGRAPHY/GEOMETRY` (SQL Server), OpenGIS‑Typen (MySQL), `SDO_GEOMETRY` (Oracle)
    

---

> 💡 Tipp: Für Portabilität früh festlegen, **welcher Dialekt** Zielsystem ist (z. B. PostgreSQL), und Dialekt‑Sonderfälle (ENUM, JSON, Zeitstempel) bewusst designen.
