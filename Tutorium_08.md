26.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 8.1: Speicher vs. Speicher

![[Pasted image 20230629191450.png]]

*Allgemein;* 
Hauptspeicher:
- Arbeitet mit logischen und physikalischen Adressen
- benutzt eher Paging
- muss schnelle Zugriffe machen
	- deswegen benutzen wir Hardware (MMU)
- Jeder langsamer Zugriff lässt die CPU in Leerlauf
	- wenn dies häufig passiert, wird das ganze System langsamer

Festplatte:
- Arbeitet mit Segmente
- Positionen innerhalb einer Datei in Blöcke auf der Festplatte
	- Aus Blatt 1, die Verzeichnisse und Dateien in einer Datei / Verzeichnis, sind nun Adressen / Verweise, wo die wirklich Datei ist.


**Lösung: **

*a)*
Hauptspeicher:
- Viel in Hardware umsetzten um die Zugriffe zu beschleunigen (MMU)
- Muss schnell erfolgen, also wenig Zeit ist da
- Die Instruktionen (also z.B. angefragte Seiten in einem String) werden die Reihe nach bearbeitet
- Verzögerung führt zu einem CPU Leerlauf.

Festplatte:
- Viel in Verwaltungsaufwand
- Zeit ist nicht mehr kritisch
- Zugriffe können langsamer behandelt werden, die Zeit wird eher in Verwaltung verbracht
	- Auch wenn der Zugriff selber mal weniger Zeit bekommt
- Umsetzung in Software
- Instruktionen bzw. Reihenfolge kann beliebig geändert werden, wenn es schneller geht.


*Allgemein zur b)*
- Defragmentierung, ist genau wie es sagt. Wir "lösen" die Fragmente bzw. Segmente unseres Speicher.
- Wie erwähnt finden Speicherbelegungen in Form von Segmente statt, das heißt jeder Prozess bekommt so viel wie er braucht.
- Angenommen, wir benutzen dies, und geben unsere Prozesse Speicher ab. 
- Irgendwann terminieren einige dieser Prozesse, und der Belegte Speicher ist dann freigegeben
- allerdings, entstehen dadurch Lücken unseren Speicher, die eventuell separat nicht groß genug sind für einen neuen Prozess, aber zusammen schon. Deswegen benutzen wir hier Defragmentierung.

![[Pasted image 20230629203335.png]]

*b)*
Bei Defragmentierung sortieren wir den Speicher erneut, zwei Gründe warum wir es machen sind:
1. Lücken schließen
	1. wie erwähnt Lücken zwischen den Prozessen werden geschlossen, 
	2. Wir kriegen dann größere Lücken für später.
2. Zusammenhängender Blöcke können dann zusammen geordnet werden
	1. Wenn Prozess X einen Block am Anfang vom Speicher hat, und einen zweiten Block in der Mitte, dann wird die Zugriffzeit natürlich höher da man den Start und das Ende von beiden Blöcken speichern muss, und natürlich auch durch den Speicher suchen müssen.
	2. Außerdem wird dadurch der Verwaltungsaufwand hoher, was wir immer verringern wollen.
	3. Also hier kommt es dazu, dass wir schnellere Zugriffzeiten haben
	4. Und Verwaltungsaufwand verringert sich

Warum benutzen wir das in dem Hauptspeicher nicht?:
- Im Hauptspeicher benutzen wir eher Paging, Defragmentierung ist eher für Segmente dedacht
- Arbeit erfolgt mit RAM, also Random Access Memory: also Zugriffe sind in der tat spontan, es gibt keine Suche von vorne nach Hinten
- Die Belegung im Hauptspeicher ändert sich durchgehend, also es lohnt sich das ganze nicht zu sortieren weil es bleibt wahrscheinlich für nur paar Momente sortiert, bevor die nächste Anfrage schon ankommt


### Aufgabe 8.2: I-Nodes


![[Pasted image 20230629191614.png]]

*Allgemein:*
Alles in Linux ist eine Datei
Sogar I/O Geräte sind auch durch Dateien (I-Node) beschrieben

- Index-Nodes: Werden in Unix verwendet
- Beinhalten Meta-Informationen über Dateisystemeinträge.
	- I-Node-Nummer
	- Typ
	- Hardlinkcount
	- Zugriffsrechte
	- Größe der Datei
	- Position der eigentlichen Dateien im Speicher
	- etc.

Gegeben:
- Typen
	- Directory
	- File
	- Symbolic Link
- Symbolic Link vs Hard Link
	- Symbolic ist eher eine Art Shortcut: also der beschreibt nur einen Verweis, wo die Datei sich wirklich befindet
	- Hard Link: Ist hart copy paste, die Datei bzw. I-Node selber wird kopiert und darein getan

Hardlink counter beschreibt hier nicht wie viele Verbindungen ein Node X hat ausgegangen von Node X, sondern wie viele andere Nodes sind mit dem Verbunden, also wie viele andere Nodes Hardlinks haben zu diesen Node


**Lösung: ** 

1. mkdir /home/bus/inodes
2. ln -s /home/bus/inodes /home/bus/inodes/inodes
3. ln /home/bus/test /home/bus/inodes/inodes/inodes/inodes/test2

Zunächst mal vielleiche eine Veranschaulichung:
Unsere Nodes stehen in dieser Verbindung zueinander
![[Pasted image 20230629200002.png]]

- Wir führen die erste Operation

![[Pasted image 20230629200056.png]]
Wir haben einen neuen Directory erzeugt "Inodes", der an /home/bus verbunden ist

![[Pasted image 20230629200253.png]]

- Zweite Operation:
- Wir erstellen hier einen Link "ln", dieser Link soll Symbolic Link sein "-s"
- Der Link verbindet /home/bus/inodes mit /home/bus/inodes/inodes

![[Pasted image 20230629200428.png]]

- Der Pfeil von Inode916 zur Inode612, beschreibt den Link den wir erstellt haben, also wenn man auf Inode916 zugreift, dann landet man bei Eltern-Verzeichnis also Inode612

![[Pasted image 20230629200743.png]]

- Dritte Operation:
- Wir erstellen erneut einen Link, aber diesmal einen Hardlink, also wir kopieren den I-Node quasi
- Der Inode ist "Inode370" ("/Bus"), der soll jetzt in  /home/bus/inodes/inodes/inodes/inodes/test2 sein
	- Mit dem Name / Verweis test2 Also:

![[Pasted image 20230629201338.png]]

- Wir erhöhen den HL Count auf zwei, da jetzt zwei I-Nodes auf 370 zeigen
- Egal wie oft wir in "/inodes" gehen, gehen wir eine Stufe zurück, also I-Node ist dann an Inode: 612 

![[Pasted image 20230629201444.png]]

*Beachtet bitte, die Bäume sind nur von mir zur Veranschaulichung, ich finde es sehr leicht sich dadrin zu verlieren. Sie sind also NICHT Teil der Lösung, und werden von euch nicht erwartet.*

### Aufgabe 8.3: Disk-Scheduling

![[Pasted image 20230629191626.png]]

*Allgemein:*
Eine Festplatte hat:
- Eine drehbare Scheibe
- Schreib-/ Lesekopf

Wie schnell gelesen wird, wird entschieden durch: 
- Wo der Head sich gerade befindet
- Rotation der Scheiben bis zweiten Sektor
	- Sektoren sind dort wo der Kopf liest

Wichtig ist auch:
- Wie der Kopf sich bewegt
- Wir haben gesagt, dass die Reihenfolge der Anfragen nicht eingehalten werden muss, wenn es schneller gehen kann, also wenn man schneller alle Anfragen beantworten kann.

Der Kopf kann nach unterschiedlichen Strategien sich bewegen, die in Der Vorlesung kamen sind:

- FCFS
	- First Come First Serve: Die Anfragen werdeneinfach die Reihenfolge nach bearbeitet 
- SSTF
	- Shortest Seek Time First: Die am nächsten Anfrage zum Kopf wird als nächstes bearbeitet (Ich liebe Deutsch)
	- Die Kopf sucht also zwischen den Anfragen welche näher ist, und bearbeitet sie, dort sucht er erneut die nächste die im dichtesten zu ihm ist.
- SCAN (C-SCAN):
	- Kopf geht von Startposition in einer Richtung und bearbeitet alle Anfragen auf dem weg, erst wenn der Kopf das Ende erreicht dann geht in die anderen Richtung und macht das gleiche bis er die Schreibe erreicht, also Ende von innen, dann wiederholt er das ganze
	- C-SCAN ist sehr ähnlich, da arbeitet der Kopf nur in einer Richtung, das heißt nachdem er das Ende erreicht hat am Anfang, geht er dann in die andere Richtung, aber *bearbeitet keine Anfragen*, die Bearbeitung erfolgt nur in einer Richtung
- LOOK (C- LOOK)
	- Analog zum Scan, die einzige Änderung ist, dass der Kopf hier nicht ganz bis zum Ende der Platte geht, sondern schon beim letzten Anfrage die Richtung wechselt (C-Look ist genau wie C-Scan, Arbeit ist nur in einer Richtung)

**Lösung: **

Gegeben:
- 500 Zylinder, von 0 bis 4999
- 2350 Startposition
- Anfragen: 3500, 4200, 3900, 1900, 2600, 1000, 4700

i) FCFS:  die Reihenfolge nach:
3500, 4200, 3900, 1900, 2600, 1000, 4700


ii) C-SCAN: Wir bewegen den Kopf aufsteigend:
2600, 3500, 3900, 4200, 4700, *4999, 0*, 1000, 1900

4999, 0, ist unsere Richtungswechsel in der Phase, wo wir nichts bearbeiten.

iii) LOOK: Richtungswechsel erst bei letzten Anfrage, und wir arbeiten in beide Richtungen
2600, 3500, 3900, 4200, 4700, 1900, 1000

### Aufgabe 8.4: I/O-System

![[Pasted image 20230629191641.png]]

*Allgemein:*

Polling vs. Interrupts:
- Interrupts kennt ihr schon: 
	- Die CPU bekommt gesagt, wenn irgendwas passiert wo sie unbedingt Zeit für geben muss
- Polling ist wenn die CPU selber nachguckt ob alles in Ordnung ist, oder ob irgendwas bearbeitet werden soll noch.

Beispiel aus dem Tutorium, ihr habt das bestimmt erlebt:
(Tutor ist hier die CPU)
- Wenn ihr eine Frage habt, dann unterbricht ihr den Tutor und *fragt ihn* schnell
	- Das ist  Interrupt, da er mit was anders beschäftigt war, und eine Frage kam
- Wenn aber eine Aufgabe zu Ende ist Zum Beispiel, dann *fragt euch der Tutor* ob ihr Fragen habt.
	- Das ist dann Polling, da er selber nach Fragen gesucht hat

**Lösung:**

*a)*
- Polling ist sinnvoll wenn:
	- Anfragen häufig vorkommen und nur kurz dauern
		- mit einem Polling Loop kann man das ganze schnell bearbeiten und zusammenliegen
		- Man vermeidet Overhead Interrupts, also vermeiden von zu vielen Interrupts
	- Bei I/O Geräte wo es egal ist ob man etwas warten muss
		- Drucker Zum Beispiel: es ist relativ egal, ob man jetzt eine Sekunde warten muss bis die Anfrage durch geht

*b)*
i) Maus:
- Buffer kann nötig sein
- hält Mausbewegungen fest, während wichtigere Anfragen bearbeitet werden
- Interrupt ist hier am besten geeignet
	- Mausbewegungen können unregelmäßig sein, jedoch sollen sofort bearbeitet werden
ii) Festplatte:
- Buffer ist nötig
	- Die Daten werden auf dem Weg von und zur Festplatte zwischengespeichert
	- Interrupts ist hier besser geeignet
		- wir können nicht sagen wann die Dateien fertig geladen / geschrieben sein werden
		- wir wissen auch nicht, wo der Lese-/ Schreibkopf sich momentan befindet
	- Polling ist auch unnötig, da die Anfragen nicht häufig vorkommen würden, aber Polling-Loop selber Zeit verschwindet

*c)*


### Aufgabe 8.5: DNS

![[Pasted image 20230629191654.png]]