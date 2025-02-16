---
#theme: simple
separator: "\n\n\n\n"
verticalSeparator: "\n\n\n"
---


# C



## Kompilierte Sprache

```mermaid
flowchart LR
    code["`.c-Dateien`"]
    compiler
    executable
    code --> compiler
    compiler --> executable
```

```bash
# compile
$ gcc -o getting_started main.c
# run
$ ./getting_started
```



## Grundstruktur

```C
// Kommentare
/*
Auch im Block/über mehrere Zeilen möglich
*/

// includes
#include <stdio.h> // für z.B. printf()

// execution starts at main()
int main(void) // Funktion
{ // {} umfassen mehrere Anweisungen zu einem Block
    printf("%s\n", "Hallo Welt"); // Funktionsaufruf
    //  der ";" beendet die Anweisung
}
```

Referenz zur Ausgabe (am Terminal): https://en.cppreference.com/w/c/io/fprintf



## Anweisungen und Ausdrücke

* Ausdruck (expression)
* Anweisung (statement)
  * Kann als Block zusammengefasst werden


### Ausdruck (expression)

```C
4 + 5
a < b
42
(23 - 2) * 2
```

* Kombination aus _Operatoren_ und _Operanden_. Kann ausgewertet werden und dabei einen Wert ergeben.

https://en.cppreference.com/w/c/language/expressions


### Anweisung (statement)

```C
a = 1 + 2;
; // leere Anweisung
{ // Block-Start
  b = a;
  c = 3 * b;
} // Block-Ende
```

* Mit `;` abgeschlossen, oder mehrere mit `{}` zu einer Block-Anweisung zusammengefasst.
* Die Zuweisung `c = 1 + 2` ist auch ein Ausdruck (mit dem Wert 3).

https://en.cppreference.com/w/c/language/statements



## Operatoren

* Arithmetische Operatoren 
* Zuweisungen
* Inkrement/Dekrement
* Logik
* Vergleich
* Zugriff
* Andere


### Arithmetische Operatoren

```C
a + b; a - b; // Summe, Differenz
a * b; a / b; a % b; // Produkt, Quotient, Rest
~a; a & b; a | b; a ^ b; // Komplement (NICHT), UND, ODER, XOR
a << b; a >> b; // Nach links schieben, nach rechts schieben
#include <iso646.h>
compl a; a bitand b;// etc.
```

* Die Operator-Rangfolge ist mathematisch sinnvoll (Punkt vor Strich).
* Dennoch immer empfehlenswert: Klammern setzen!

https://en.cppreference.com/w/c/language/operator_alternative
https://en.cppreference.com/w/c/language/operator_arithmetic


### Zuweisung

```C
a = 1;
b = b + 2;
b += 2; // identisch zu b = b + 2;
```

* `=`, von rechts nach links
* und Kombinationen mit allen arithmetischen Operatoren: `+=`, `-=`, etc.

https://en.cppreference.com/w/c/language/operator_assignment


### Inkrement/Dekrement

```C
++a; --a; // Prefix
a++; a--; // Postfix
b = 2 * a++; // zuerst b = 2 * a, dann wird a erhöht
a = a++; // undefined
```

* Die betroffene Variable nicht mehrmals in einem Ausdruck verwenden!

https://en.cppreference.com/w/c/language/operator_incdec


### Logische Operatoren

```C
!a; // Nicht
a && b; // UND
a || b; // ODER
#include <iso646.h>
not a; a and b; // etc.
```

* Logisch falsch: 0
* Logisch wahr: von 0 verschieden
  * Zwei wahr-Werte können unterschiedlich sein!
* Nicht mit den bitweisen Operatoren verwechseln!

Notiz: [iso646.h bei cppreference.com](https://en.cppreference.com/w/c/language/operator_alternative)
https://en.cppreference.com/w/c/language/operator_logical


### Vergleiche

```C
a == b; a != b; // Gleich, ungleich
a < b; a > b; // Kleiner, größer
a <= b; a >= b; // Kleiner gleich, größer gleich
```

* Logisch falsch: 0
* Logisch wahr: von 0 verschieden
* Achtung: `=` und `==` nicht verwechseln!

https://en.cppreference.com/w/c/language/operator_comparison


### Zugriff

```C
&a; // Speicheradresse von a
*b; // Inhalt des Speichers mit der Adresse in b
a.d; // Struktur-Mitglied d
b->d; // Struktur-Mitglied d, b enthält eine Adresse
c[i]; // Array-Element mit Offset i
```

* `&` und `[]` verwenden wir in der 1. Klasse
* Die Übrigen kommen in der 2. Klasse dazu
 
https://en.cppreference.com/w/c/language/operator_member_access


### Andere Operatoren

```C
sizeof(int); // Anzahl an Bytes für eine Variable des Typs int
sizeof a; // Anzahl an Bytes für den Wert des Ausdrucks a
a, b; // Zuerst wird a ausgewertet, dann b.
      // Der gesamte Ausdruck hat den Wert von b.
(int) a; // a wird zum Typ int umgewandelt
a ? b : c; // Wenn a wahr ist wird b ausgewertet, sonst c
```

* Den ternären Operator `a ? b : c` sparsam einsetzen!
  * Die Lesbarkeit des Codes leidet ggf. sehr.

https://en.cppreference.com/w/c/language/operator_other



## Variablen

```C
int baz = 23; // baz ist eine ganze Zahl, ab hier 23
```

* Variablen sind ein Name für Platz im Speicher
* Der Typ definiert wie viel Platz benötigt wird
  * `int` ist meistens 4 Bytes (32 bits) groß
* Der Typ definiert wie der Platz zu interpretieren ist
  * Man kann den Speicher bei Bedarf auch anders interpretieren (cast)


### Sichtbarkeit und Lebensdauer

```C
int a; // Sichtbarkeit: global
static int b; // Sichtbarkeit: Datei
{
  int c; // Lebensdauer: Block, Sichtbarkeit: Block
  static int d; // Lebensdauer: Global, Sichtbarkeit: Block
}
```

* Globale Variablen sparsam einsetzen!

https://en.cppreference.com/w/c/language/scope


### Deklaration, Definition, Initialisierung

```C
extern int foo; // Deklaration
int bar; // Definition
int baz = 23; // Initialisierung
```


#### Deklaration

```C
extern int foo; // Deklaration
// Ab hier ist foo bekannt (und hat einen Datentyp)
```

* Reserviert keinen Speicherplatz.
* Legt nur Typ und Namen fest.
* Kann wiederholt vorkommen.

https://en.cppreference.com/w/c/language/declarations


#### Definition

```C
int bar; // Definition
```

* Reserviert auch den Speicherplatz
  * Bei Funktionen den Inhalt (kommt später)


#### Initialisierung

```C
int a = 23; // Initialisierung
const int b = 23; // Initialisierung
                  // b kann nicht mehr verändert werden
```

* Bestimmt den (ersten) Inhalt
  * Vorher ist der Inhalt (meist) nicht bekannt.
* Variablen bei der Definition _immer_ initialisieren!

https://en.cppreference.com/w/c/language/const



## Basisdatentypen

* Bool'sche Werte
* Ganze Zahlen
* Gleitkommazahlen
* Zeichen



## Exkurs: Zahlensysteme

* Wir verwenden üblicherweise _Stellenwertsysteme_
* siehe: https://de.wikipedia.org/wiki/Stellenwertsystem


### Stellenwertsysteme

* Im Dezimalsystem (Basis $b = 10$): Einer, Zehner, Hunderter, ...
  * Geschrieben in umgekehrter Reihenfolge
*  $123 = 1 \cdot 10^2 + 2 \cdot 10 + 3 \cdot 1$
* $Z = a_2 \cdot b^2 + a_1 \cdot b^1 + a_0 \cdot b^0$


### Allgemeiner Fall

* Für $N$ Stellen: $Z = \sum_{i=0}^{N-1} a_i \cdot b^i$
* Anzahl an darstellbaren Werten: $b^N$
* Zahlenbereich: $0 \dots b^N - 1$


### Binärsystem

* PCs rechnen im Binärsystem ($b=2$)
* Platz ist endlich, z.B. 1 Byte = 8 bit
  * $ Z = \sum_{i=0}^{7} a_i \cdot b^i$
  * 256 Werte: 0...255
* Darstellung am Bildschirm/im Code ist länglich
  * Andere Basis für die Darstellung notwendig
  * Seit C23 mit `0b`-Präfix möglich


### Oktalsystem

* Basis $b = 8$ (3 bit)
* Historisch wichtig
* Mit `0` als Präfix
  * Achtung Stolperfalle führende Null bei Dezimalzahlen!
  * `010` ist 8, nicht 10!
* Demo: `ping`-Kommando 


### Hexadezimalsystem

* Basis $b = 16$ (4 bit, ein halbes Byte, 1 nibble)
* Symbole 0...9, A...F (a...f) mit Werten 0...15
* Genutzt wenn
  * die binäre Darstellung eine Rolle spielt
  * die Darstellung als Text unabhängig vom Wert gleich lang sein soll
* Mit `0x` oder `0X` als Präfix
* Demo: git commit numbers


### Beispiele

* `64` ist auch `0100` oder `0x40`
  * und ggf. `0b1000000`
* `127` ist auch `0177` oder `0x7f` bzw. `0x7F`
  * und ggf. `0b1111111`
* `0x1F` ist $1 \cdot 16 + 15 \cdot 1 = 31$


### Negative Zahlen

* Im Zweierkomplement dargestellt
  * Alle Bits invertieren und 1 addieren
* Subtraktion wird zur Addition
* Anzahl an darstellbaren Werten: $2^N$
* Zahlenbereich: $-2^{N-1} \dots 2^{N-1}-1$


### Gleitkommazahlen

* $Z = s \cdot m \cdot b^e$
  * $s$ ... Vorzeichen
  * $m$ ... Mantisse ($1 < m < b$)
  * $b$ ... Basis
  * $e$ ... Exponent

https://de.wikipedia.org/wiki/Gleitkommazahl


### Eigenschaften von Gleitkommazahlen

* Die relative Genauigkeit ist über den gesamten Wertebereich gleich.
  * Die absolute Genauigkeit variiert
  * Nicht für Geldbeträge verwenden!
* Mit $b=2$ (wie am PC) wird $1/10$ periodisch
  * Es kommt bei anderen Zahlen zu Rundungsfehlern als mit dezimalen Gleitkommazahlen



## Basisdatentypen für Zahlen

* Bool'sche Werte
* Ganze Zahlen
  * mit und ohne Vorzeichen
* Gleitkommazahlen
  * unterschiedliches Fassungsvermögen


### Bool'sche Werte

```C
#include <stdbool.h>
bool a = true;
```

* Wahr oder Falsch
* Zwei Literale
  * `true` oder `false`
* eigentlich ganze Zahlen
  * `false` ist 0, alle anderen Werte sind _nicht_ `false` (`true` ist nur _einer von vielen_)
  * Niemals auf Gleichheit mit `true` prüfen!


### Ganze Zahlen

* mit Vorzeichen
  * optionales `signed` vor dem Datentyp, der Default
* ohne Vorzeichen
  * `unsigned` vor dem Datentyp
* Literale
  * Wenn man einen konkreten Wert im Code angeben möchte
  * z.B. bei der Initialisierung


#### Ganze Zahlen: unbestimmtes Fassungsvermögen

```C
char a = 96;               // mind. 8 bit, sizeof(char) ≥ 1
short int b; short c = 42; // mind. 16  bit
int d = 23;                // mind. 16 bit
long int e; long f = 5L    // mind. 32 bit
long long int g; long long h = 7LL; // mind. 64 bit

unsigned char i; // etc. wie oben, aber nur pos. Zahlen
``` 
https://en.cppreference.com/w/c/language/arithmetic_types


#### Ganze Zahlen: definiertes Fassungsvermögen

```C
#include <stdint.h>
int8_t a; int16_t b; int32_t c; int64_t d; // exakt 8, ... bits
int_fast8_t e; // mindestens 8 bit, die schnellste Variante
int_least8_t f; // mindestens 8 bit, die kleinste Variante

uint8_t g; // etc. wie oben, aber nur pos. Zahlen
``` 
https://en.cppreference.com/w/c/types/integer


#### Wertebereichsgrenzen

```C
#include <limits.h>
long a = LONG_MAX; // je nach Compiler und Platform
long b = LONG_MIN; // je nach Compiler und Platform

unsigned long c = ULONG_MAX; // je nach Compiler und Platform

// etc.
```
https://en.cppreference.com/w/c/types/limits


### Gleitkommazahlen

```C
float a = 0.5f; // 0.5
double b = 0.5e3; // 500
long double c = 0.5e4L; // 50000
```

https://de.wikipedia.org/wiki/IEEE_754
https://en.cppreference.com/w/c/language/floating_constant


#### Besondere Werte

```C
#include <math.h>
double a = -5;
double b = 0;
double c = a/b; // -inf, siehe INFINITY
double d = a/c; // 0
double e = b/b; // -nan, siehe NAN
double f = atan(INFINITY); // pi/2, weil entlang der y-Achse
```

https://en.cppreference.com/w/c/header/math


#### Wertebereichsgrenzen

```C
#include <float.h>
float a = FLT_MIN; // kleinster, positiver Wert
double b = DBL_MAX; // größter, positiver Wert
double c = DBL_EPSILON; // Differenz zwischen 1.0 und dem nächsten darstellbaren Wert
```

https://en.cppreference.com/w/c/header/float
https://en.cppreference.com/w/c/types/limits#Limits_of_floating-point_types


#### Dezimalstellen

```C
#include <float.h>
// text -> float -> text
FLT_DIG; // typ. 6
DBL_DIG; // typ. 15
LDBL_DIG; // typ. 18
// float -> decimal -> float
FLT_DECIMAL_DIG;  // min. 6 (typ. 9)
DBL_DECIMAL_DIG;  // min. 10 (typ. 17)
LDBL_DECIMAL_DIG; // min. 10 (typ. 21)
```

https://en.cppreference.com/w/c/header/float
https://en.cppreference.com/w/c/types/limits#Limits_of_floating-point_types



## Exkurs: Zeichen-Codierung

* Computer kennen keine Glyphen, nur Zahlen
* Eine Codierung weist einem Glyphen eine Zahl zu und umgekehrt
* ASCII ist historisch weit verbreitet
  * 0...127 standardisiert
  * kann per se keine Umlaute etc. (128...255)
* Unicode ist heutzutage die Zuordnung von Glyphen zu Zahlen
  * Zahlen zu Bytefolge meist UTF-8
  * 0...127 ident zu ASCII

https://de.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange


### Steuerzeichen

* Die Zeichen 0...31 in ASCII sind Steuerzeichen
  * selbst nicht darstellbar
  * beeinflussen ggf. die Darstellung


* Genaue Bedeutung hängt vom Umfeld ab
  * 10/0x0a/"\n"/LF ist ein Zeilenumbruch unter Unix/Linux/BSD/macOS
  * unter MS-DOS und Windos braucht es CRLF (13,10/0x0d,0x0a/"\r\n")
  * Programmausgabe unter Windows braucht nur LF für einen Zeilenumbruch, das
    Terminal macht automatisch aus jedem LF ein CRLF
  
https://de.wikipedia.org/wiki/Steuerzeichen


### Beispiele

* 'A' ist U+0041 (dez. 65)
  * ASCII und UTF-8 ident
  * LATIN CAPITAL LETTER A


* 'Ä' ist U+00C4 (dez. 196)
  * In ASCII nicht vorhanden
  * LATIN CAPITAL LETTER A WITH DIAERESIS
  * in UTF-8 die Bytefolge 0xc3 0x84 (195, 132)


* 'Ä' ist U+0041 gefolgt von U+0308
  * LATIN CAPITAL LETTER A
  * gefolt von COMBINING DIAERESIS
  * in UTF-8: 0x41 0xcc 0x88 (65, 204, 136)


* '🖉' ist U+1F589
  * LOWER LEFT PENCIL
  * in UTF-8: 0xf0 0x9f 0x96 0x89 (240, 159, 150, 137)

https://www.unicode.org/charts/


### Verarbeitung

* Encodings müssen in der gesamten Verabeitungskette berücksichtigt werden.
* Wenn überall UTF-8 eingesetzt wird funktioniert alles
  * Nicht überall wird UTF-8 eingesetzt
* Demo: VS Code "Change File Encoding"



## Basisdatentypen für Zeichen

```C
char a = 'A';
printf("%c\n", a); // A
printf("%d\n", a); // 65
```

https://en.cppreference.com/w/c/language/character_constant
https://en.cppreference.com/w/c/language/arithmetic_types
https://en.cppreference.com/w/c/io/fprintf


## Steuerzeichen in C

```C
'\x0a', '\0' // LF, NUL
'\a', // audible bell
'\t', // tab
'\n', // LF
'\r', // CR
'\\', '\'' // \ '
..
```

https://en.cppreference.com/w/c/language/escape



## Ein- und Ausgabe
```C
#include <stdio.h>

int main(void) {
  int a = 0;
  scanf("%i", &a); // & nicht vergessen!
  printf("%i\n", a); // hier kein &
}
```

Format String: https://en.cppreference.com/w/c/io/fscanf



## Allgemeine Quellen

* [cppreference.com](https://en.cppreference.com/w/c)
* [C17/C18 final draft](https://files.lhmouse.com/standards/ISO%20C%20N2176.pdf)
* [Wikibooks: C Programmierung](https://de.wikibooks.org/wiki/C-Programmierung)
