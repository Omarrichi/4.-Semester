### Aufgabe 1:

 **a)**
 Vorwissen:
- Die CPU kann nur einen einzigen Prozess durchlaufen lassen.
- Ziel ist eine Art "Multitasking", damit mehrere Prozess schnelle nacheinander durchlaufen
	- wegen der höhen Rechen- und Wechselgeschwindigkeit kommt es den Benutzer so vor, dass die CPU Prozesse gleichzeitig ausführt.

Damit Prozesse gestoppt werden  und andere ausgeführt werden können, sind Interrupts nötig, sonst würde man warten bis die CPU jeden Prozess komplett ausgeführt hat.

Interrupt:
Wenn ein Interrupt gerufen wird, wird eine "Routine" durchbearbeitet, sodass der Prozess selber nicht merkt, dass er gestoppt wird, und damit die CPU genau den alten Prozess ausführt wo er vorher gestoppt wurde.

---

**Lösung:**

ISR (Interruptserviceroutine):
1. Zunächst wird der Programmcounter auf den Stack gespeichert
	- Der PC gibt an, wo wir im uns im Code befinden und wo wir weiter machen.
2.  Danach wird der PC auf die ISR gesetzt (Also ja, die ISR ist ja fast ein normales Programm der ausgeführt werden muss)
3. die ISR wird weiter ausgeführt, und schaltet Interrputs erstmal aus, damit wir nicht ein Interrput während eines Interrputs bekommen
4. Danach wird der Kontext von dem unterbrochenen Prozess auf dem Stack gespeichert.
	- Also die Inhalte der Register in der CPU (z.B. Variabelen) werden dann auf dem Stack festgehalten, damit sie durch die ISR nicht geändert werden.
5. in der Regel, resettet ISR den Hardware
6. ggf. führt die ISR Interrupt-Code aus
7. Nach dem beenden von dem Code, wird der alte Prozess wiederhergestellt. Also Kontext wird von dem Stack wieder in die Registern geladen
8. Interrputs werden dann wieder freigeschaltet, und der PC counter wird dann wieder auf die alte Position in dem Prozess eingesetzt, und der Code wird weiter ausgeführt

---

**b)**

**Lösung:**
- Jede Datei in Linux ist ein Objekt gespeichert auf der Festplatte, diese Objekte enthalten den Inhalt der Datei.
- Ein Verzeichnis ist eine Sammlung von Objekten (also Datein), aber selber wiederrum eine Datei
	- Verzeichnise enthalten aber *nicht* die Dateien selber.
	- Sie enthalten Meta-Informationen z.B. Ordnernamen, und Referenzen also Verweise auf die Dateien, die das Verzeichnis enthält.

![[Pasted image 20230424192343.png]]
- Beispiel für ein Verzeichnis Baum:
Die Files (1,2,3) sind in Dir(1,2,3) entsprechend zusehen, in Wirklichkeit aber sind in Dir(1,2,3) lediglich Vorweise auf diese Files nicht die Files selber.
- Wichtig auch in UNIX, werden I/O Geräte (Tastatur, Bildschrim, etc.) auch durch den Inter-Prozess Kommunikation auf "Datein" abgebildet, die dann über einen Filedescriptor zugegriffen werden können.

---

**c)**

Vorwissen:

- Systemcalls (Syscalls) sind Methoden, die bereitgestellte Funktionalitäten in dem Betriebsystem aufrufen. 
- Dabei wird die Kontrolle vom Programm an den Kernel übergeben.
- Was ist ein Filedescriptor:
	- Nach dem öffnen einer Datei, wird ein Filedescriptor zurückgegeben.
	- Dieser repräsentiert die geöffnete Da