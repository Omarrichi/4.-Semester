05.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

#### Aufgabe 6.1 Speicherverwaltung:

a) Welche Aufgaben hat die Speicherverwaltung in Betriebssystemen?

b) Welche Aufgaben hat die MMU? Welchen Sinn hat dabei die Zweiteilung einer logischen Adresse in Segement- bzw. Seitennummer und Offset?

c)  der Aufruf einer logischen Adresse, zu der keine physikalische Adresse existiert, vorkommen? Wenn ja, wie sollte die MMU damit umgehen?

d) Warum ist es sinnvoll, die MMU als Hardwarekomponente umzusetzen, anstatt die notwendigen Berechnungen in Software durchzuführen?

**Lösung:**

a) 

- Übersetzten von logischen in physikalischen Adressen
- Zuweisung von Speicher an Prozesse, Freigabe von Speicher
- Verhinderung von unerlaubten Speicherzugriffen
- Auslagerung von Speicher auf bzw. Laden vom Hintergrundspeicher
- Ermöglichung von Shared Memory

b) 

- Die MMU rechnet logische Adressen in physikalische um
- befindet sich zwischen CPU und Speicher
- Durch die Zweiteilung wird der Umfang der Abbildungstabelle der MMU reduziert
	- Muss nicht zu jeder möglichen Adresse ein Eintrag in der Tabelle existieren, sondern nur jeder möglichen Adresse eines Segments / Seite.

c)

- darf vorkommen, es ist auch ein Regelfall also kommt regelmäßig vor.
- Es heißt, ein Prozess will auf eine Speicherstelle zugreifen, die aber nicht in den Hauptspeicher geladen ist
- Page Fault: Die MMU übersetzt und sucht die angefragte Adresse, und sie wird dann in den Hauptspeicher geladen.

d) 
- Geschwindigkeit, übersetzten passiert sehr häufig, deswegen extra Komponente, sonst wird das System Allgemein verlangsamt

#### Aufgabe 6.2 Adressen:

Für die Speicherverwaltung nach dem Segmentierungsverfahren sei für einen Prozess die folgende Segmenttabelle gegeben:

| Segment | Basisadresse | Länge |
|:-------:|:------------:|:-----:|
|    0    |     1320     |  320  |
|    1    |     562      |  58   |
|    2    |     310      |  120  |
|    3    |     750      |  60   |
|    4    |     830      |  150  |
|    5    |     990      |  110  |

Sowohl Basisadresse als auch Länge seien in Byte angegeben.

a) Wie viele Byte stehen dem Prozess im physikalischen Speicher zur Verfügung?

b) Die folgenden logischen Adressen werden nun von dem Prozess angefragt. Was würde die MMU jeweils zurückgeben?

1. (3,30)
2. (1,18)
3. (2,122)
4. (4,149)

**Lösung:**

a) Aussummieren der Einträge der Spalte "Länge": 
$320+58+120+60+150+110= 818$

b) 

1. (3,30) : 780
	1.  Segment 3 und Offset 30. Wir prüfen, dass der Offset die Länge nicht überschreitet wenn ja dann Segmentation Fault, wenn nein, dann Basisadresse + Offset zurückgeben.
2. (1,18): 580
3. (2,122): Offset $\geq$ Länge, die MMU würde den Zugriff unterbinden und ein Seg. Fault würde auftreten
4. (4,149): 979

#### Aufgabe 6.3 Buddysystem:

Gegeben sei ein Rechner mit einem Hauptspeicher von 32 MByte. Der Hauptspeicher werde mit einem gewichteten Buddy-System verwaltet. In diesem System erfolgen nun in der angegebenen Reihenfolge
Speicheranforderungen und -freigaben:

| Nr. |  Operation  | Name | Größe in MByte |
|:---:|:-----------:|:----:|:--------------:|
|  1  | Anforderung |  A   |       8        |
|  2  | Anforderung |  B   |       12       | 
|  3  | Anforderung |  C   |       8        |
|  4  |  Freigabe   |  A   |                |
|  5  | Anforderung |  D   |       4        |
|  6  | Anforderung |  E   |       5        |
|  7  |  Freigabe   |  D   |                |
|  8  | Anforderung |  F   |       3        |

Führen Sie die Operationen, die in der obigen Tabelle angegeben sind, mittels eines gewichteten BuddySystems durch. Halten Sie den Zustand des Hauptspeichers nach jeder Operation in Form eines binären Baums fest. Falls möglich, werden Knoten im Verhältnis 1 : 2 geteilt, sonst 1 : 3. Bei der Wahl zwischen zwei gleichberechtigten Ästen wählen Sie stets den linken aus. Ordnen Sie ihren binären Baum so, dass sich kleine Werte links und große Werte rechts befinden.
Warum eignen sich Buddy-Systeme für die Speicherverwaltung?

**Lösung:**

![[Pasted image 20230607223636.png]]

#### Aufgabe 6.4 Segmentierung vs. Paging:

geben sei ein PC mit 16 GiB Hauptspeicher und 64 Bit Adressraum. Sie überlegen, ob Sie Segmentierung oder Paging einsetzen sollen. Zur Auswahl stehen:

- Paging mit Seiten von 4 Byte Größe
- Paging mit Seiten von 256 MiB Größe
- Segmentierun mit Best Fit als Strategie zur Suche nach freiem Speicher

Welche Vor- und Nachteile haben die drei Varianten in Bezug auf Verwaltungsaufwand, interne Fragmentierung und externe Fragmentierung?

*Allgemein:*

- Adressraum k Bit: $n = 2^k$ mögliche logische Adressen (hier: $2^{64}$)
- Adressen werden in Seiten der Größe s unterteilt: $\frac{n}{s}$
- Seiten werden in Rahmen der Größe h gelegt: $\frac{h}{s}$ 

Interne Fragmentierung: 
- Tritt bei Paging auf: 
	- Der Seiten haben eine feste Größe, wenn ein Prozess wenig Speicher braucht, bekommt er trotzdem eine komplette Seite, obwohl sie viel großer ist, als was er wirklich braucht
	- Der unbenutzte Speicher in dieser Seite wird dann verloren

Externe Fragmentierung: 
- Tritt bei Segmentation auf:
	- Die Segments sind nicht fest, die können beliebig groß sein, also jeder Prozess bekommt genau so viel wie er braucht
	- jedoch die Reihenfolge der Segmente und die Positionierung im Speicher, kann dazu führen, dass zwischen den Segmente Lücken entstehen, die zu klein sind.
	- Ein Prozess kann Speicheranfragen, der in der Tat frei ist auch, aber der freie Speicher ist in Lücken unterteilt zwischen den Segmente, deswegen kann der Prozess keinen Speicher bekommen.


**Lösung:**

- Paging mit Seiten von 4 Byte Größe
	- Eine Seite ist s = $2^2$ Byte groß
	- Also $\frac{n}{s} = \frac{2^{64}}{2^2} = 2^{62}$ Einträge in der Tabelle
	- Speicher ist nicht groß genug, wir müssen mehrstufige Tabellen benutzen (7 Stufen)
	- Zu viele Stufen, Verzögerung für die Adressumsetzung

- Paging mit Seiten von 256 MiB Größe
	- Verwaltungsaufwand deutlich geringer
	- Seitentabelle hat $2^{36}$ Einträge, Mit drei oder vierstufigen Paging ginge das noch
	- interne Fragmentierung

-  Segmentierung mit Best Fit Strategie:
	- Verwaltungsaufwand im Mittel
	- externe Fragmentierung

Fazit: Es gibt keine optimale Lösung, in der Praxis mittelgroße Seitengroße Seitengrößen werden gewählt, und Implementierung wird mit Segmentierung vermischt.


#### Aufgabe 6.5 Speicherverwaltung:

a) Zwei Prozesse verwenden ein Shared Memory zur Kommunikation. Verwenden beide Prozesse die gleichen logischen Adressen, um auf das Shared Memory zuzugreifen?

b) Welchen Vorteil für die Speicherverwaltung hat es, vorzuhalten, ob eine Seite bisher nur gelesen wurde?

c) Welchen Vorteil hat ein Copy-on-Write bei der Erzeugung eines Kindprozesses mittels fork?

d) Warum ist es nicht sinnvoll, sehr komplexe Algorithmen zu entwickeln, um die Seitenfehlerrate zu verringern?

**Lösung:**

a) 
- Ja und nein