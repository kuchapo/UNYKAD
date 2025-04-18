# Datentypen

```mermaid
graph LR
    A[Datentypen]

    %% Hauptaufteilung
    A1[Intrinsische Typen]
    A2[Nicht-intrinsische Typen]

    A --> A1
    A --> A2

    %% Intrinsische Typen
    A1a[Arithmetische Typen]
    A1b[Sonstige intrinsische Typen]
    A1c[Abgeleitete Typen]

    A1 --> A1a
    A1 --> A1b
    A1 --> A1c

    %% Arithmetische Untertypen
    A1a1[Integrale Typen]
    A1a2[Fließkomma-Typen]

    A1a --> A1a1
    A1a --> A1a2

    %% Integrale Typen
    A1a1 --> I1[int]
    A1a1 --> I2[short]
    A1a1 --> I3[long]
    A1a1 --> I4[long long]
    A1a1 --> I5[char]
    A1a1 --> I6[bool]
    A1a1 --> I7[wchar_t]
    A1a1 --> I8[char16_t]
    A1a1 --> I9[char32_t]

    %% Fließkomma-Typen
    A1a2 --> F1[float]
    A1a2 --> F2[double]
    A1a2 --> F3[long double]

    %% Weitere intrinsische Typen
    A1b --> B1[void]
    A1b --> B2[std::nullptr_t]

    %% Abgeleitete Typen
    A1c --> D1["T* (Zeiger)"]
    A1c --> D2["T& (Referenz)"]
    A1c --> D3["T[] (Array)"]

    %% Nicht-intrinsische Typen
    A2 --> U1[class]
    A2 --> U2[struct]
    A2 --> U3[union]
    A2 --> U4[enum]
    A2 --> U5[typedef / using]
    A2 --> U6["template&lt;class T&gt;"]
```

!!! info
    Die Typen int, short, long, long long und char gibt es als `signed` oder `unsigned`.
    Standardmäßig sind sie signed, also für positive und negative Zahlen gedacht.
    Mit unsigned lassen sich nur positive Zahlen speichern – aber in einem größeren Bereich.
    Zum Beispiel: Ein signed char (1 Byte) kann Werte von –128 bis 127 speichern,
    ein unsigned char von 0 bis 255.

## Intrinsische (primitive) Datentypen

Typen, die vom Compiler eingebaut sind und zur Sprache selbst gehören:

| **Datentyp**          | **Beschreibung**                            | **Speichergröße**               |
|-----------------------|---------------------------------------------|---------------------------------|
| `int`                 | Ganze Zahlen                                | 4 Bytes                         |
| `short`               | Kleine ganze Zahlen                         | 2 Bytes                         |
| `long`                | Große ganze Zahlen                          | 4 oder 8 Bytes*                 |
| `long long`           | Sehr große ganze Zahlen                     | 8 Bytes                         |
| `char`                | Einzelne Zeichen (signed oder unsigned)     | 1 Byte                          |
| `bool`                | Wahrheitswerte (`true` / `false`)           | 1 Byte                          |
| `wchar_t`             | Breitere Zeichen (plattformabhängig)        | 2 oder 4 Bytes*                 |
| `char16_t`            | UTF-16 Zeichen                              | 2 Bytes                         |
| `char32_t`            | UTF-32 Zeichen                              | 4 Bytes                         |
| `float`               | Gleitkommazahl (einfache Genauigkeit)       | 4 Bytes                         |
| `double`              | Gleitkommazahl (doppelte Genauigkeit)       | 8 Bytes                         |
| `long double`         | Präzisere Gleitkommazahl                    | 8, 12 oder 16 Bytes*            |
| `void`                | Kein Wert                                   | Keine Speichergröße             |
| `std::nullptr_t`      | Typ von `nullptr`                           | Wie ein Zeiger (4 oder 8 Bytes) |
| `T*`                  | Zeiger auf beliebigen Typ                   | 4 oder 8 Bytes*                 |
| `T&`                  | Referenz auf einen Typ                      | Wie Zieltyp (Referenzmodell)    |
| `T[]`                 | Array eines Typs                            | Abhängig von der Länge          |

---

## Nicht-intrinsische (abgeleitete) Datentypen

Typen, die du selbst definierst:

| **Datentyp**         | **Beschreibung**                              | **Speichergröße**                     |
|----------------------|-----------------------------------------------|---------------------------------------|
| `class`              | Benutzerdefinierte Klasse                     | Abhängig von enthaltenen Mitgliedern  |
| `struct`             | Struktur (ähnlich wie `class`, aber `public`) | Abhängig von enthaltenen Feldern      |
| `union`              | Speicher für mehrere Typen (nur einer aktiv)  | Größe des größten Elements            |
| `enum`               | Aufzählung fester benannter Konstanten        | Typabhängig, oft wie `int` (4 Bytes)  |
| `typedef` / `using`  | Alias für bestehenden Typ                     | Wie Zieltyp                           |
| `template<class T>`  | Generischer Typ, bei Benutzung konkretisiert  | Wie spezifizierter Zieltyp            |


---

**Hinweise:**  
Speichergrößen mit einem Sternchen (`*`) können je nach **Systemarchitektur** variieren:  
  - **32-Bit-Systeme**: Hier sind Speichergrößen oft kleiner, z. B. `long` = 4 Bytes, Zeiger = 4 Bytes.  
  - **64-Bit-Systeme**: Hier sind Speichergrößen größer, z. B. `long` = 8 Bytes, Zeiger = 8 Bytes.  
Die Größe von `long double` kann ebenfalls je nach Compiler und Plattform variieren (z. B. 8, 12 oder 16 Bytes).
