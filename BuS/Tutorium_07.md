19.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

Tut mir leid für die Verspätung, da aber das Thema etwas komplex sein kann, und viele wegen DSAL Einsicht nicht da seien konnten, habe ich mich entscheiden die Notizen ausführlicher zu machen.


Dieses Blatt ist Teil 2 des Kapitels "Speicherverwaltung"

Zunächst **Allgemeines:**

Wenn ein Prozess ausgeführt wird, werden entsprechend seine Daten geladen dies geschieht indem die Seiten in den Hauptspeicher geladen werden. Jedoch hat der Hauptspeicher nicht genug Platz für alle diese Seiten für alle ausgeführten Prozesse, deswegen werden nur die nötigen Seiten in den Hauptspeicher geladen. Wenn ein Prozess  ausgeführt wird dann müssen seine Seiten in die CPU geladen werden, dies geschieht über die MMU.

- Bei Zugriff auf einer Seite:
	- Die MMU prüft, ob diese Seite im Hauptspeicher schon exisitert.
	- Wenn ja, dann wird sie nun übersetzt (Physikalisch $\Leftrightarrow$ Logisch)
	- Wenn nein, dann:
		- 1. Die Seite existiert tatsächlich, muss aber von der Festplatte in den Hauptspeicher geladen werden (Page Fault / Seitenfehler)
		- 2. Die Seite existiert nicht bzw. gehört diesen Prozess nicht, dann wird der Zugriff verhindert (Segmentation Fault)
- Wenn ein Seitenfehler vortritt dann: 
	- Behandlungsroutine ausführen (Page Fault Handler)
	- Prozess ggfs. schlafen legen
	- Fehlende Seite wird lokalisiert und nachgeladen (eventuell werden andere Seiten verdrängt!)
- Seitenfehler verbringen einiges an Zeit deswegen: Seitenfehler Vermeiden!

### Aufgabe 7.1: Paging

```ad-note
title:Allgemein

Demand-Paging: beschreibt das System Speicher zu verwalten, sodass Speicher nur dann zugegriffen wird, wenn es gebraucht ist. Auf unserem System, holen wir uns also nur die Seiten die wir brauchen, und verdrängen die, die wir nicht mehr brauchen würden.
Das System ist nicht optimal implementierbar, da man nicht wissen kann, wann welche Seite angefragt werden wird, deswegen behandeln wir Allgemein, Strategien um eine Approximation zu erreichen.
```

Gegeben sei ein Demand-Paging-System, das aktuell wie folgt ausgelastet ist:

- CPU-Auslastung: 20%
- Auslastung des Zugriffs auf die Paging-Disk (d.h. die Festplatte, auf die der virtueller Speicher ausgelagert ist): 97,7%
- Auslastung der sonstigen I/O-Geräte: 5%

Ziel sollte aber eigentlich sein, die CPU so gut wie möglich auszulasten, um alle Programme schnell abzuarbeiten! Geben Sie für jeden der folgenden Vorschläge mit Begründung an, ob er sich eignen
würde, die Auslastung ihrer CPU zu verbessern (d.h. zu erhöhen):

i) Installation einer schnelleren CPU

ii) Installation einer größeren Festplatte

iii) Erhöhung der Anzahl laufender Prozesse

iv) Verringerung der Anzahl laufender Prozesse

v) Installation von mehr Hauptspeicher

vi) Installation einer schnelleren Festplatte

**Lösung:**

1. hilft nicht, unser CPU ist jetzt schon nicht ausgelastet, eine schnellere wird nun weniger ausgelastet sein, die Geschwindigkeit Seiten zu laden ändert sich hier nicht.
2.  Hilft auch nicht, Speicherplatz ist kein Problem, Problem ist die Geschwindigkeit.
	1. Ihr könnt euch vorstellen: Wenn unser Seiten Objekte in einem Lager sind, und die Tür ist so groß wie eine normale Tür, wenn wir den Lager größer machen, heißt nicht automatisch, dass wir eine größere Tür haben (Oder Schuhe auf Running Modus :) ) 
3. Mehr Prozesse, heißt mehr Seiten Anfragen, heißt mehr Seitenfehler, also hilft auch nicht
4. Analog wie drei jedoch umgekehrt, weniger Prozesse, heißt weniger Anfragen, also weniger Seitenfehler, dies wird also helfen.
5. Mehr Hauptspeicher, heißt mehr Platz für unsere Seiten, also wir werden weniger Seiten austauschen müssen $\Rightarrow$ weniger Seitenfehler, also das wird auch helfen
6. Das wird helfen, zwar haben wir immer noch die gleiche Anzahl an Seitenfehler, aber die CPU kann die Daten schneller bekommen
	1. Das gleiche wie bei 2. Lager bleibt diesmal gleiche große, jedoch die Tür wird größer, sodass man schneller hin und her laufen kann, (Running Modus ist hier auch gut treffend)


### Aufgabe 7.2: Demand Paging

```ad-abstract
title: Zusammenfassung
unten ist eine Wand Text, hier ist eine kleine Zusammenfassung:
Wir wollen Seiten laden, es gibt insgesamt 7 Seiten [0,1,2,3,4,5,6]
Wir haben eine begrenzte Anzahl an Plätze im Hauptspeicher (4 Rahmen)
Also wir können nicht alle Seiten gleichzeitig einfach laden, wir wissen nun welche als nächstes geladen werden muss, müssen jedoch entscheiden welche wir rausschmeißen wollen

Liest euch die Beschreibungen der Strategien aus der Frage, bei der Lösung findet ihr auch eine  Erklärung.
```


In der Vorlesung haben Sie das Prinzip des Pagings kennengelernt: der Adressraum eines Prozesses wird in Seiten (Pages) fester Größe eingeteilt, der Hauptspeicher in Rahmen der gleichen Gröÿe, welche jeweils eine Seite aufnehmen können. Die Zahl der Rahmen im Hauptspeicher ist im Allgemeinen kleiner als die Anzahl der tatsächlich im System existierenden Seiten. Seiten, die gerade nicht im Hauptspeicher gehalten werden können, werden auf ein externes Speichermedium ausgelagert. Befindet sich eine Seite nicht im Hauptspeicher, wenn auf sie zugegriffen werden soll, muss sie zuvor vom externen Speicher in den Hauptspeicher geladen werden (Seitenfehler, Page Fault). Sind noch nicht alle Rahmen im Hauptspeicher gefüllt, stellt dies kein Problem dar. Ist jedoch kein Platz mehr vorhanden, muss eine Seite im Hauptspeicher ausgewählt werden, die ausgelagert und durch die neue Seite ersetzt wird. Für die Auswahl der zu ersetzenden Seite gibt es verschiedene Strategien, unter anderem:

**FIFO (First In, First Out):** Die jeweils älteste Seite wird aus dem Hauptspeicher ausgelagert.

**LRU (Least Recently Used):** Es wird die Seite aus dem Hauptspeicher entfernt, auf die am längsten kein Zugriff mehr erfolgt ist.

**SC (Second Chance):** Diese Strategie ist eine Vereinfachung von LRU. Pro Seite wird ein ACCESSED Bit gespeichert, welches gesetzt wird, wenn auf eine bereits geladene Seite ein weiterer Zugriff erfolgt. Muss eine Seite ersetzt werden, wird die älteste Seite geprüft: ist ihr A-Bit nicht gesetzt, wird sie verdrängt; ist ihr A-Bit gesetzt, wird sie zur jüngsten Seite und ihr A-Bit wird zurückgesetzt - nun wird erneut die jetzt älteste Seite überprüft. Dies geschieht so lange, bis eine Seite verdrängt wurde.

**LFU (Least Frequently Used):** Es wird die Seite mit der geringsten Nutzungshäufigkeit ausgetauscht. Die Nutzungshäufgkeit bezieht sich dabei auf ein Zeitintervall der jüngsten Vergangenheit. Für diese Aufgabe wird angenommen, dass die Nutzungshäufigkeit jeweils ab dem Ladezeitpunkt einer Seite gezählt wird. Liegt für mehrere Seiten die gleiche Nutzungshäufigkeit vor, wird die älteste dieser Seiten zuerst ausgetauscht.

**CLIMB:** Wird eine Seite geladen, wird sie als 'älteste' Seite vermerkt. Wie bei FIFO erfolgt stets eine Verdrängung der ältesten Seite. Eine Seite muss sich zuerst bewähren, um länger im Speicher verbleiben zu können: erfolgt ein erneuter Zugriff auf eine Seite, während sie bereits im Speicher ist, wird sie um eine Position verjüngt, also bezüglich ihres Alters mit der nächstjüngeren Seite getauscht.

**OPT (Optimalstrategie):** Diese Strategie ersetzt die Seite, die am längsten nicht mehr benötigt wird. Wird eine Seite in Zukunft gar nicht mehr benötigt, wird sie auf die Stelle mit niedrigster Priorität gesetzt.
Seitenzugriffe werden hier als Referenzstring angegeben, welcher die zeitliche Reihenfolge der angeforderten Seiten enthält. Ein solcher String ist z.B. $\omega$ = 0 1 2 3 4 1 5 5 2 4 5 1 1 0 6 2 5 2 3 5 : Erst erfolgt ein Zugriff auf Seite 0, dann auf Seite 1, dann auf Seite 2 usw.. Seitennummern seien hier immer einstellig.

Gegeben sei ein System mit vier Rahmen, die zu Beginn leer sind. Geben Sie für jede der oben aufgeführten Strategien die Belegung dieser vier Rahmen nach jedem Seitenzugriff sowie die Anzahl der auftretenden Seitenfehler an. Legen Sie dabei den oben angegebenen Referenzstring zugrunde.

Sollten beim Laden einer Seite noch mehrere Rahmen frei sein, belegen Sie immer den Rahmen mit der niedrigsten Nummer. Ist kein Rahmen mehr frei, ersetzen Sie jeweils eine Seite entsprechend der verwendeten Strategie.

Dabei sollen die Felder "Priorität" der Unterstützung und Übersichtlichkeit bei der Erstellung der Rahmenbelegung dienen (füllen Sie sie auf jeden Fall auch aus). Diese Felder entsprechen der Liste, die das Betriebssystem verwaltet, um zu entscheiden, in welcher Reihenfolge bereits geladene Seiten ausgetauscht werden. Wird z.B. bei FIFO als erstes Seite 5 geladen, so ist "Priorität 1" 5 und die anderen Felder sind leer. Wird im zweiten Schritt 7 geladen, so ist "Priorität 1" 7, "Priorität 2" 5 und die anderen Felder sind leer. Der Wert in Priorität 4 gibt also die Seite an, die beim nächsten Seitenfehler ausgetauscht wird. Halten Sie in diesem Feld auch das A-Bit für SC und und den Häufigkeitszähler für LFU nach.

Die einzige Ausnahme bei der Sortierung der Prioritätenliste soll bei LFU gemacht werden: verwenden Sie wie bei FIFO nur das Alter der Seiten als Sortierkriterium. Dies führt zwar dazu, dass  je nach Ständen der Häufigkeitszähler - die zu verdrängende Seite nicht immer in "Priorität 4" steht sondern ggfs. über den zusätzlichen Zähler entschieden werden muss, macht aber das Beibehalten der korrekten Reihenfolge bei der Änderung von Zählerständen einfacher. 
Im Feld "Seitenfehler" markieren sie mit einem X, wenn in diesem Schritt ein Seitenfehler aufgetreten
ist und eine Seite ausgetauscht wurde.

**Lösung:**

Wichtig hier bei allen Strategien, wenn eine Seite verdrängt wird, werden die Rahmen nicht komplett neu zugeteilt, lediglich übernimmt die neue Seite den Platz der älteren Seiten. Aus diesem Grund führen wir eine zweite Tabelle "Priorität", sodass wir mithalten können welche jetzt als nächstes verdrängt werden soll. Prioritätstabelle, ist in der Regel keine Pflicht, jedoch ist sie extrem hilfreich, ich empfehle euch sie immer mitzuführen, da sie auch Folgefehler ermöglicht.

- FIFO:
	- Die einfachste Strategie, jede neue Seite wird als jüngste im Hauptspeicher eingelagert, wenn alles voll ist, dann nehmen wir die älteste aus dem Speicher.

![[Pasted image 20230622131130.png]]

- LRU (Least Recently Used): 
	- Identisch zur FIFO mit dem einzigen Unterschied:
	- Falls eine Seite im Hauptspeicher ist, bzw. Falls kein Page Fault vorliegt, wird die angefragte Seite als jüngste vermerkt.
	- Die älteste wird ausgelagert

![[Pasted image 20230622131437.png]]

- SC (Second Chance):
	- Fast so wie LRU, falls die angefragte Seite schon vorhanden ist, dann wird sie nicht direkt als jüngste vermerkt, sondern ihr A-Bit wird gesetzt (also auf 1).
	- Wenn wir eine Seite verdrängen müssen, jedoch die älteste hat ihr A-Bit gesetzt, dann bekommt diese Seite eine zweite Chance. 
	- (hier analog zu LRU), die Seite verliert ihren A-Bit und wird als jüngste vermerkt, danach wird die zweit älteste Seite untersucht, falls sie kein A-Bit gesetzt hat, dann wird sie verdrängt, falls ja dann läuft es analog wie bei der ersten Seite.
	- Also diese Strategie ist LRU mit einem zwischen Schritt, und mehrere Seiten gleichzeitig, können ihr A-Bit gesetzt haben.
	- Wichtig hier auch:
		- Falls eine Seite A-Bit verliert und jung wird bekommt sie Prio 1, aber sie wird um eine Stelle nachunten verdrängt da das Laden von der angefragte Seite erst danach passiert, also bekommt unser A-Bit Verlierer Platz 2, und neue Seite bekommt Platz 1.

![[Pasted image 20230622132258.png]]
A-Bit halten wir als Index bei jeder Seite.

- LFU (Least Frequently Used):
	- Etwas komplex, neue Seiten werden als jüngsten vermerkt (wie FIFO).
	- Jede Seite bekommt einen Zähler, jedes mal wenn die Seite angefragt wird, erhöht sich dieser Zähler.
	- Wenn wir uns entscheiden müssen welche Seite verdrängt werden muss dann:
		- wir nehmen die Seite die den niedrigsten Zähler hat
		- Falls mehrere den Minimum besitzen, dann nehmen wir die ältere Seite raus.
	- Falls eine Seite verdrängt wird, und dann erneut Angefragt wird, fängt ihr Zähler erneut von 0 an
	- Hier ist es euch überlassen ob ihr bei 0 oder 1 anfangen wollt zu zählen, Hauptsache es bleibt gleich.

![[Pasted image 20230622133106.png]]
Index hier ist die Anzahl der Nutzungen
- CLIMB:
	- Wichtig: Neue Seite hier ist älteste *nicht* Jüngste
	- Analog zur LRU, Unterscheid ist, wenn eine Seite benutzt wird, wird sie nicht als jüngste vermerkt, sondern "jünger" also sie geht eine stelle höher nur (sie klettert eine Stelle hoch)
	- Hier wird wieder einfach die älteste Seite verdrängt.

![[Pasted image 20230622133509.png]]

- Optimal:
	- Unser Ziel ist diese Strategie zu haben, jedoch diese Strategie setzt voraus, dass wir welche Seiten angefragt werden wissen. Dies ist bei dieser Aufgabe möglich jedoch in der Realität nicht. Wir wissen nicht welche Seite als nächstes angefragt wird, deswegen haben wir mehrere Strategien. Diese helfen uns eine Approximation zu haben.
	- Wir gucken bei dieser Strategie auf unseren String in einem bestimmten Intervall, und verdrängen die Seite, die in diesen kommenden Intervall nicht angefragt wird.
	- diese folgt aus dem Working-Set-Prinzip und Lokalitätsprinzip
		- wir nehmen uns also ein Teil des Strings und arbeiten damit
	- Die anderen Strategien arbeiten auch natürlich ähnlich, (SC, LFU, LRU) jedoch übernehmen sie ihre Werte aus vergangenen Anfragen, hier gucken wir nach vorne

![[Pasted image 20230622134344.png]]

Wir haben diese Strategie hier, damit ihr sieht, dass keiner von den anderen wirklich in diesen Fall optimal ist, jedoch einige sind besser als andere, und manche sind Implementierungsaufwändiger als andere (LRU oder LFU), deswegen wird eher SC und Clock benutzt, sie sind eine einfache Variante dieser Strategien. In Linux wird eine Variante von Clock benutzt.

### Aufgabe 7.3: Lifetime-Funktion

**Allgemin:**
Hier wird nur das Vorgehen für die Aufgabe beschrieben mehr zum Thema Lifetime-Function ist auf Folie V-84

```ad-note
- Wenn wir eine bekannte String haben, dann können wir Tests durchführen, indem wir Rahmen zuweisen, fangen bei einem Rahmen, und gehen bis 10 Rahmen. 
- Nun ermitteln wir so, wie viele Seitenfehler wir bei jedem Rahmenanzahl haben.
- Jetzt können wir die mittlere Zeit zwischen zwei Seitenfehler für jeden Rahmenanzahl rausfinden.
	- wir teilen die Dauer der Ausführung des Referenzstrings durch die Anzahl der Seitenfehler.
		- Hier wird jede Anfrage als eine Zeiteinheit betrachtet, unser String hat  die Länge 26, also wir Teilen 26 durch die Seitenfehler Anzahl bei jede Rahmenzahl
- Das Ergeniss der Division ist dann $L(m)$
	- diese Tragen wir in der Diagramm, und legen unsere Tangente
- Die Tangent ist bei Punkt (0,1), diese lassen wir dann einfach nach rechts fallen, bis sie bei einem Punkt ankommt, Die Rahmengröße für diesen Punkt, ist dann unsere Optimale Rahmengröße

```



Gesucht wird die Lifetime-Funktion $L(m)$ zu einem Programm, das durch den Referenzstring $\omega$ mit

$$\omega = 2 \space 3\space 5\space 3\space 1\space 4\space 2\space 3\space 1\space 5\space 2\space 3\space 4\space 0\space 5\space 1\space 4\space 0\space 3\space 2\space 5\space 1\space 2\space 5\space 2\space 1\space$$


gekennzeichnet ist. Die Zeit, die zwischen zwei Seitenanfragen vergeht, soll jeweils eine Zeiteinheit betragen. Die Seitenersetzung erfolge mittels LFU. Gehen Sie davon aus, dass das Working Set zu Beginn leer ist. Geben Sie die Zahl der Seitenfehler für $m = 1 \dots 10$ in einer Tabelle an, berechnen Sie für jede Rahmenzahl m die mittlere Zeit zwischen zwei Seitenfehlern und zeichnen Sie den Graphen der Lifetime-Funktion $L(m)$ für $m = 1 \dots 10$.

![[Pasted image 20230622134841.png]]

Welche optimale Einstellung der Speichergröße ergibt sich bei Anwendung des primären Knie-Kriteriums? Zeichnen Sie zusätzlich die zugehörige Gerade in Ihren Graphen ein.
Nutzt Linux dieses Konzept? Warum oder warum nicht?

**Lösung:**

Gegeben: 
- Referenzstring: $\omega = 2 \space 3\space 5\space 3\space 1\space 4\space 2\space 3\space 1\space 5\space 2\space 3\space 4\space 0\space 5\space 1\space 4\space 0\space 3\space 2\space 5\space 1\space 2\space 5\space 2\space 1\space$
- Rahmen m von 1 bis 10
- Strategie: LFU

Als nächstes würde man hier tatsächlich 10 Tabellen erstellen müssen, damit man weiß wie viele Seitenfehler wir haben. Tabelle 1 hat nur einen Rahmen, Tabelle 2 2 Rahmen, Tabelle 5 5 Rahmen usw..

Da diese Tabellen tatsächlich eher als Hilfe gedacht sind und nicht für die Aufgabe Pflicht, können wir uns hier etwas Arbeit sparen.

- Wir fangen bei dem einfachen Fall:
	- Unser String beinhaltet Seiten von 0 bis 5, also insgesamt haben wir 6 Seiten.
	- Das heißt, wenn wir 6 Rahmen haben, dann können wir tatsächlich alle Seiten Laden, ohne das wir verdrängen müssen.
		- Nicht vergessen aber, wenn wir die Seiten zum ersten Mal laden, ist es ein Page Fault
	- Also bei 6 Rahmen, werden wir genau 6 Page Faults haben. Wir laden alle Seiten einmalig, und dann sind wir fertig.
	- Für die restlichen Rahmen größer 6, ist es analog, wir haben zwar mehr Rahmen, das macht aber keinen Unterscheid für unsere Seiten, sie können geladen werden und müssen nicht verdrängt werden.
	- Also folgt also:
	- bei Rahmen 7,8,9,10: 6 Pagefaults
- Jetzt habe wir fast die hälfte abgedeckt, ohne eine einzige Tabelle
- Weiterhin können wir für Rahmen 1 auch ohne Tabelle zählen:
	- Bei einem Rahmen werden wir jedes mal die Seite verdrängen, es sei, die gleiche Seite wird zwei mal hintereinander ausgeführt. 
	- Hier ist das nicht der Fall, keine Seite kommt direkt zwei mal vor, also wir haben für jede Anfrage einen Page Fault
	- 26 Anfragen = 26 Page Faults

also haben wir jetzt folgendes:

| Rahmengröße | Page Fault |
|:-----------:|:----------:|
|      1      |     26     |
|      2      |            |
|      3      |            |
|      4      |            |
|      5      |            | 
|      6      |     6      |
|      7      |     6      |
|      8      |     6      |
|      9      |     6      |
|     10      |     6      |

Wenn ihr geschickt seid, könnt ihr weiterhin ohne Tabelle die 2 und vielleicht auch die 3 machen, das ist aber bei LFU etwas komplex, deswegen würde man für den Rest Tabellen erstellen. Als Übung könnt ihr gerne dies machen, im folgenden könnt ihr dann die Anzahl an PF vergleichen

| Rahmengröße | Page Fault |
|:-----------:|:----------:|
|      1      |     26     |
|      2      |     22     |
|      3      |     21     |
|      4      |     13     |
|      5      |     9      |
|      6      |     6      |
|      7      |     6      |
|      8      |     6      |
|      9      |     6      |
|     10      |     6      |

Jetzt können wir endlich die Aufgabe machen :) .

Wir rechnen $L(m)$.

$m = 1 , L(m) = \frac{26}{PF} = \frac{26}{26} = 1$
$m = 2 , L(m) = \frac{26}{22} = 1,18$
$m = 3 , L(m) = \frac{26}{21} = 1,23$
$m = 4 , L(m) = \frac{26}{13} = 2$
$m = 5 , L(m) = \frac{26}{9} = 2,88$
$m = 6 , L(m) = \frac{26}{6} = 4,33$
$m = 7 , L(m) = \frac{26}{6} = 4,33$
$m = 8 , L(m) = \frac{26}{6} = 4,33$
$m = 9 , L(m) = \frac{26}{6} = 4,33$
$m = 10 , L(m) = \frac{26}{6} = 4,33$

Nun tragen wir diese Werte in unserem Diagramm ein:

![[Pasted image 20230622142318.png]]

Dazu zeichnen wir die Tangente $(0,1)$ und lassen sie fallen, sie kommt dann an den Punkt $m = 6$

Also unsere Optimale Speichergröße bzw. Rahmengröße ist 6

Sowas wird aber in Linux nicht eingesetzt, da wir nicht einmalig die Werte rechnen können, diese Werte ändern sich stets, deswegen müssen wir die Werte ständig neu ermitteln. Wir wissen auch gar nicht in welcher Reihenfolge die Seiten angefragt werden, also ist es nicht nur aufwändig, sondern auch so gut wie unmöglich.

Stattdessen haben wir in Linux PFF, wir geben einem Prozess immer einen festen wert an Rahmen. Während der Lebenszeit dieses Prozesses berechnen wir dann die Seitenfehlerrate, wird sie zu hoch, dann teilen wir weitere Rahmen zu, damit wir diese absenken, falls es sehr niedrig ist, und andere Prozesse mehr Speicherbenötigen also mehr Rahmen benötigen, dann werden Rahmen abgegeben von diesem Prozess. Wir passen uns also während der Laufzeit an, falls eine Änderung im Programm vorkommt, dann fangen wir erneut, die Situation zu evaluieren, und erneut anzupassen.

```ad-warning
title: Aufgabe 4 behandelt wir dann nächste Woche
```