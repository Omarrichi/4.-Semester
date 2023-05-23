22.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 5.1: Resource Allocation Graph

Betrachten Sie in dieser Aufgabe das Problem der Erkennung von Deadlocks. Gegeben sei ein System mit drei Prozessen $P1, . . . , P3$ und vier Betriebsmitteln $A, . . . ,D$, von denen jeweils nur ein Exemplar zur Verfügung steht und die nur von einem Prozess gleichzeitig verwendet werden können. Die
Anforderungen von Betriebsmitteln durch die Prozesse ist in der folgenden Tabelle gegeben.

![[Pasted image 20230523223544.png]]

Stellen Sie die Situation aus der obigen Tabelle für den Zeitpunkt $t = 6$ anhand eines Resource Allocation Graph dar. Liegt ein Deadlock vor? Begründen Sie Ihre Antwort.

**Lösung 5.1:**

Zunächst brauchen wir eine Hilfstabelle, hier findet ihr beide Tabellen, vom Tutorium und von der Musterlösung.

*Musterlösung:* 

![[Pasted image 20230523223822.png]]

(W: wartet, \*: Prozess braucht das Betriebsmittel noch N Zeiteinheiten, ist aber gerade blockiert, D: Done, R: Running)

*Eigene Tabelle:*

| Zeit | $P_1$     | $P_2$          | $P_3$     |
| ---- | --------- | -------------- | --------- |
| 1    | D(2)      | B(4)           | -         |
| 2    | D(1)      | B(3)           | A(5)      |
| 3    | C(3)      | B(2),D(3)      | A(4)      |
| 4    | C(2)      | B(1),D(2),c(2) | A(3),b(3) |
| 5    | C(1),a(5) | B(1),D(2),c(2) | A(3),b(3) |
| 6    | C(1),a(5) | B(1),D(2),c(2) | A(3),b(3) | 

- Hier beschriebt jede Spalte den Verlauf eines Prozesses
- Wenn wir X(N) schreiben dann:
	- Betriebsmittel X ist dem Prozess P gegeben worden, und es muss für N Zeiteinheiten noch dort bleiben
- Wenn wir x(N) schreiben dann:
	- Betriebsmittel *kann* nicht an Prozess P übergeben werden, da er zur Zeit einem anderen Prozess gehört.
- groß geschrieben heißt P hat X
- klein geschrieben heißt P braucht x aber x ist bei einem anderen Prozess schon.
- Wenn ein Prozess ein x (also klein geschriebener Betriebsmittel) in seinen Spalte hat, dann ist dieser Prozess blockiert (blockiert ist wenn der Prozess *nichts* ausführt, entsprechend lässt er die anderen Betriebsmitteln, die er hat, nicht frei).