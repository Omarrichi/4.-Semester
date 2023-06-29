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
- Im H.Speicher benutzen wir eher Paging, Defragmentierung ist eher für Segmente dedacht
- Arbeit erfolgt 
### Aufgabe 8.2: I-Nodes

![[Pasted image 20230629191614.png]]

### Aufgabe 8.3: Disk-Scheduling

![[Pasted image 20230629191626.png]]

### Aufgabe 8.4: I/O-System

![[Pasted image 20230629191641.png]]

### Aufgabe 8.5: DNS

![[Pasted image 20230629191654.png]]