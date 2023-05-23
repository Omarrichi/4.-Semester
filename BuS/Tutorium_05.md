22.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 5.1: Resource Allocation Graph

Betrachten Sie in dieser Aufgabe das Problem der Erkennung von Deadlocks. Gegeben sei ein System mit drei Prozessen $P1, . . . , P3$ und vier Betriebsmitteln $A, . . . ,D$, von denen jeweils nur ein Exemplar zur Verfügung steht und die nur von einem Prozess gleichzeitig verwendet werden können. Die
Anforderungen von Betriebsmitteln durch die Prozesse ist in der folgenden Tabelle gegeben.

![[Pasted image 20230523223544.png]]

Stellen Sie die Situation aus der obigen Tabelle für den Zeitpunkt $t = 6$ anhand eines Resource Allocation Graph dar. Liegt ein Deadlock vor? Begründen Sie Ihre Antwort.

**Lösung 5.1:**

Die 4 Bedingungen eines Deadlocks sind:

- Circular Wait: Jeder Prozess wartet auf einen anderen Prozess graph gesehen ist es ein Zykel (Alle Prozess sind blockiert)
- Exclusive Use: Es gibt nur einen Exemplar für jedes Betriebsmittel, und dieses darf nur von einem Prozess gleichzeitig benutzt werden.
- Hold and Wait: Wenn ein Prozess blockiert wird, dann lässt er die ihm zugewiesenen Betriebsmitteln, bis er nicht mehr blockiert ist, und für N Zeiteinheiten läuft
- Non-Präemtiv: Der Prozess bestimmt selber wann er die Betriebsmitteln zurückgibt.

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

Aus der Tabelle gelesen folgt der Graph:

![[Pasted image 20230523225320.png]]

- Aus dem Graph kann man sehen nach 6 Einheiten entsteht ein Kreis und alle Prozesse sind Blockiert (Circular wait)

- (Exclusive Use): Folgt aus der Frage, es gibt nur jeweils ein Exemplar eines BM, der auch nur von einem Prozess benutzt werden kann
- Außerdem aus der Frage: der Prozess setzt seine Arbeit nur dann fort, wenn ihm die angeforderten Bm zugewiesen worden sind (Hold and Wait).
- (Non Preemption): Auch aus der Frage: Der Prozess bestimmt, das BM erst wieder zu einem späteren Zeitpunkt zurückzugeben.

Alle Bedingungen sind also erfüllt, es gibt ein Deadlock.

### Aufgabe 5.2: Prozessfortschrittsdiagramme

Wollen zwei Prozesse während ihrer Laufzeit verschiedene Betriebsmittel exklusiv benutzen, könnten sie dies dem System im Voraus mitteilen, um Deadlocks zu vermeiden. Das Betriebssystem könnte dann ihren zeitlichen Ablauf so koordinieren, dass ein erfolgreiches Beenden beider Prozesse gesichert ist (falls dies möglich ist).

Zur Veranschaulichung des Ablaufs kann man Prozessfortschrittsdiagramme verwenden. Die Achsen markieren dabei die Laufzeiten der beiden Prozesse, die um Betriebsmittel konkurrieren. Schraffiert oder farblich hervorgehoben werden die Zeiträume, in denen beide Prozesse gleichzeitig ein exklusives Betriebsmittel beanspruchen. Ein Schedule gibt an, welcher Prozess in welcher Zeiteinheit ausgeführt wird (z.B. für die Zeiteinheiten 1 bis 10 und zwei Prozesse A und B: AABABBBAAA).

**a)** Gegeben seien die Prozesse A und B, die jeweils zehn Zeiteinheiten laufen, und fünf exklusiv zu nutzende Betriebsmittel BM1 bis BM5. Die Prozesse versuchen in den folgenden Zeiteinheiten ihrer Ausführung auf die Betriebsmittel zuzugreifen:

![[Pasted image 20230523230715.png]]

Tragen Sie diese Situation in ein Prozessfortschrittsdiagramm ein (Prozess A: y-Achse, Prozess B: x-Achse).

**b)** Zu jedem Zeitpunkt befindet sich das System in einem Zustand $(x, y)$, d.h. Prozess $B$ wurde bereits für $x$ Zeiteinheiten ausgeführt, Prozess $A$ für $y$ Zeiteinheiten. Ein Zustand, in dem eine erfolgreiche Beendigung der beiden Prozesse möglich ist, wird _sicher_ genannt. Ein Zustand, in dem zwar noch nicht unbedingt ein Deadlock vorliegt, von dem aus allerdings ein Deadlock unvermeidlich ist, wird _unsicher_ genannt. Ein Zustand im Diagramm, der durch keine Ausführungsreihenfolge der Prozesse jemals erreicht werden kann, wird _unmöglich_ genannt. Heben Sie in Ihrem Diagramm aus Teil a) hervor, welche Bereiche unsicher und welche unmöglich sind. Geben Sie darüber hinaus an, welchen Status (sicher, unsicher, unmöglich) folgende Zustände haben:
$$(2, 4); (3, 2); (8, 10); (6, 7)$$

**c)** Schreiben Sie einen Schedule für die beiden Prozesse auf, so dass beide Prozesse vollständig ausgeführt werden können, und zeichnen Sie ihn in das Diagramm aus Teil a) ein. 

*Hinweis*: Ein sequentieller Schedule (d.h. zunächst die vollständige Ausführung eines Prozesses,
dann die des anderen) ist an dieser Stelle verboten.

**Lösung:**

![[Pasted image 20230523231738.png]]

| Zeitpunkt | Status   |
| --------- | -------- |
| $(2,4)$   | sicher   |
| $(3,2)$   | unsicher |
| $(8,10)$  | sicher   |
| $(6,7)$   | sicher   |

### Aufgabe 5.3: Banker-Algorithmus

Gegeben seien drei Prozesse $P1, P2$ und $P3$, die drei Betriebsmittel $BM1, BM2$ und $BM3$ benutzen wollen. Es existieren $9$ Exemplare von $BM1$, $10$ von $BM2$ und $16$ von $BM3$; jedes Exemplar kann nur exklusiv genutzt werden. Zum Zeitpunkt $0$ sei der Vektor freier Betriebsmittel $(BM1, BM2, BM3)$ gegeben mit: $V(0) = (V_1(0), V_2(0), V_3(0)) = (7, 5, 11)$.

Weiterhin gilt:
- $Q_1^{max}(0) = (Q_{11}^{max}, Q_{12}^{max}, Q_{13}^{max}) = (5,3,5)$
	- $H_1(0) = (H_{11}(0),H_{12}(0),H_{13}(0)) = (0,2,1)$
- $Q_2^{max}(0) = (2,3,5), H_2(0) = (0,2,3)$
- $Q_3^{max}(0) = (5,1,1), H_3(0) = (2,1,1)$ 

**a)** Prüfen Sie mit dem in der Vorlesung vorgestellten Banker-Algorithmus zur Deadlockvermeidung, ob sich das System in einem sicheren Zustand befindet.

**b)** In der Realität erfolgen nicht alle Anforderungen auf einen Schlag, sondern es werden immer eine oder wenige Anforderungen zu einem Zeitpunkt kommen. Der Banker-Algorithmus kann auch dazu verwendet werden, zu solch einer Anforderung zu entscheiden, ob das System in einem sicheren Zustand bleibt oder nicht.

Zum Zeitpunkt $k$ erfolge nun eine Anforderung. Betrachten Sie die folgenden vier Alternativen. Welche der Anforderungen führen jeweils vom obigen Zustand ausgehend in einen sicheren Zustand, d.h welche Anforderungen dürften erfüllt werden?

1. $Q_2^{akt}(k)=(0,2,0)$
2. $Q_2^{akt}(k)=(0,0,2)$
3. $Q_3^{akt}(k)=(0,0,1)$
4. $Q_3^{akt}(k)=(0,1,0)$


*Hinweis:* Sie können an dieser Stelle wieder jeweils den gesamten Banker-Algorithmus ausführen, aber dies ist nicht unbedingt nötig; eine informelle Beschreibung mit korrekter Argumentation reicht auch aus.

**c)** Ein System verwernde den Banker-Algorithmus, um Deadlocks zu vermeiden. Zu einem betrachteten Zeitpunkt fordere ein Prozess $P$ ein Betriebsmittel $BM$ an; die Anforderung wird allerdings vom System abgelehnt. Kann man daraus sicher schließen, dass die Zuteilung von $BM$ an $P$ einen Deadlock verursachen würde? Bzw. warum nicht?

**Lösung:**

a)

Sicherheitsprüfung mit dem Banker-Algorithmus: 

1. P1 ist erster unmarkierter Prozess.
- Prüfe $Q^{max}_1(k) = (5, 3, 5) <= V(k) = (7, 5, 11)$
- Setze $V(k) := V(k) +H(k) = (7, 5, 11) + (0, 2, 1) = (7, 7, 12)$. Markiere P1.

2. P2 ist nächster unmarkierter Prozess.
- Prüfe $Q^{max}_2 (k) = (2, 3, 5) <= V(k) = (7, 7, 12)$
- Setze $V(k) = V(k) +H(k) = (7, 7, 12) + (0, 2, 3) = (7, 9, 15)$. Markiere P2.

3. P3 ist nächster unmarkierter Prozess.
- Prüfe $Q^{max}_3 (k) = (5, 1, 1) <= V(k) = (7, 9, 15)$
- Setze $V(k) = V(k) +H(k) = (7, 9, 15) + (2, 1, 1) = (9, 10, 16)$. Markiere P3.

4. Alle $P_i$ sind markiert, also ist der Zustand sicher.

b)

Betriebsmittel-Zuteilung mit dem Banker-Algorithmus

-  $Q_2^{akt}(k)=(0,2,0):$

1. $Q^{akt}_2 (k) = (0, 2, 0) <= V(k) = (7, 5, 11)$
2. Setze probehalber V(k) := V(k) −Q2(k) = (7, 3, 11)
	- H2(k) := H2(k) +Q2(k) = (0, 2, 3) + (0, 2, 0) = (0, 4, 3) Qmax
2
(k) := Qmax(k) −Q2(k) = (2, 3, 5) − (0, 2, 0) = (2, 1, 5).