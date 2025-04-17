# C++ Grundlagen

## Datentypen

### Intrinsische (primitive) Datentypen

| **Datentyp**      | **Beschreibung**                | **Speichergröße**        |
|-------------------|---------------------------------|--------------------------|
| `int`             | Ganze Zahlen                    | 4 Bytes                  |
| `short`           | Kleine ganze Zahlen             | 2 Bytes                  |
| `long`            | Große ganze Zahlen              | 4 oder 8 Bytes*          |
| `long long`       | Sehr große ganze Zahlen         | 8 Bytes                  |
| `float`           | Dezimalzahlen                   | 4 Bytes                  |
| `double`          | Dezimalzahlen                   | 8 Bytes                  |
| `long double`     | Präzisere Dezimalzahlen         | 8, 12 oder 16 Bytes*     |
| `char`            | Einzelne Zeichen                | 1 Byte                   |
| `bool`            | Wahrheitswerte (`true`/`false`) | 1 Byte                   |
| `void`            | Kein Wert                       | Keine Speichergröße      |

---

### Nicht-intrinsische (abgeleitete) Datentypen

| **Datentyp**      | **Beschreibung**               | **Speichergröße**                    |
|-------------------|--------------------------------|--------------------------------------|
| `int*`, `char*`   | Zeiger auf Speicher            | 4 oder 8 Bytes*                      |
| `int&`, `char&`   | Referenzen                     | 4 oder 8 Bytes*                      |
| `int[]`           | Arrays                         | Abhängig von Anzahl der Elemente     |
| `struct`, `class` | Benutzerdefiniert              | Abhängig von enthaltenen Variablen   |
| `enum`            | Benannte Werte                 | Typabhängig, oft wie `int` (4 Bytes) |

---

**Hinweise:**
- Speichergrößen mit einem Sternchen (`*`) können je nach **Systemarchitektur** variieren:
  - **32-Bit-Systeme**: Hier sind Speichergrößen oft kleiner, z. B. `long` = 4 Bytes, Zeiger = 4 Bytes.
  - **64-Bit-Systeme**: Hier sind Speichergrößen größer, z. B. `long` = 8 Bytes, Zeiger = 8 Bytes.
- Die Größe von `long double` kann ebenfalls je nach Compiler und Plattform variieren (z. B. 8, 12 oder 16 Bytes).

## Präzedenztabelle der Operatoren

| **Bezeichnung**          | Operatorsymbol                     | Priorität | Bewertungsreihenfolge |
| ------------------------ | ---------------------------------- | --------- | --------------------- |
| Klammern                 | () []                              | 14        | Von links nach rechts |
| Komponentenauswahl       | . ->                               | 14        | Von links nach rechts |
| Arithmetische Negation   | -                                  | 13        | Von rechts nach links |
| Logische Negation        | !                                  | 13        | Von rechts nach links |
| Bitlogische Negation     | ~                                  | 13        | Von rechts nach links |
| Inkrement                | ++                                 | 13        | Von rechts nach links |
| Dekrement                | --                                 | 13        | Von rechts nach links |
| Arithmetische Operatoren | * / %                              | 12        | Von links nach rechts |
|                          | + -                                | 11        | Von links nach rechts |
| Shift-Operatoren         | << >>                              | 10        | Von links nach rechts |
| Vergleichsoperatoren     | > >= < <=                          | 9         | Von links nach rechts |
|                          | == !=                              | 8         | Von links nach rechts |
| Bit-Operatoren           | &                                  | 7         | Von links nach rechts |
|                          | ^                                  | 6         | Von links nach rechts |
|                          | \|                                 | 5         | Von links nach rechts |
| Logische Operatoren      | &&                                 | 4         | Von links nach rechts |
|                          | \|\|                               | 3         | Von links nach rechts |
| Zuweisungsoperatoren     | = += -= *= /= %= >>= <<= &= ^= \|= | 2         | Von rechts nach links |
| Sequenzoperator          | ,                                  | 1         | Von rechts nach links |
