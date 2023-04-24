24.04.2023 - Omar Richi

Für die Abgaben:
Bitte meldet euch unbedingt in einem Abgabe Team
Es sollen Dreierteams sein, 


### Aufgabe 1.1: Betriebssystemgrundlagen

 **a)** Beschreibe die Abfolge von Ereignissen, die auftreten, wenn eine ISR durch einen HardwareInterrupt ausgelöst wird.
 
 Vorwissen:
- Die CPU kann nur einen einzigen Prozess durchlaufen lassen.
- Ziel ist eine Art "Multitasking", damit mehrere Prozess schnelle nacheinander durchlaufen
	- wegen der höhen Rechen- und Wechselgeschwindigkeit kommt es den Benutzer so vor, dass die CPU Prozesse gleichzeitig ausführt.

Damit Prozesse gestoppt werden  und andere ausgeführt werden können, sind Interrupts nötig, sonst würde man warten bis die CPU jeden Prozess komplett ausgeführt hat.

Interrupt:
Wenn ein Interrupt gerufen wird, wird eine "Routine" durchbearbeitet, sodass der Prozess selber nicht merkt, dass er gestoppt wird, und damit die CPU genau den alten Prozess ausführt wo er vorher gestoppt wurde.

**Lösung:**

ISR (Interruptserviceroutine):
1. Zunächst wird der Programmcounter auf den Stack gespeichert
	- Der PC gibt an, wo wir im uns im Code befinden und wo wir weiter machen.
2.  Danach wird der PC auf die ISR gesetzt (Also ja, die ISR ist ja fast ein normales Programm der ausgeführt werden muss)
3. Die ISR wird weiter ausgeführt, und schaltet Interrputs erstmal aus, damit wir nicht ein Interrput während eines Interrputs bekommen
4. Danach wird der Kontext von dem unterbrochenen Prozess auf dem Stack gespeichert.
	- Also die Inhalte der Register in der CPU (z.B. Variabelen) werden dann auf dem Stack festgehalten, damit sie durch die ISR nicht geändert werden.
5. In der Regel, resettet ISR den Hardware
6. Ggf. führt die ISR Interrupt-Code aus
7. Nach dem beenden von dem Code, wird der alte Prozess wiederhergestellt. Also Kontext wird von dem Stack wieder in die Registern geladen
8. Interrputs werden dann wieder freigeschaltet, und der PC counter wird dann wieder auf die alte Position in dem Prozess eingesetzt, und der Code wird weiter ausgeführt

---

**b)** In der Unix-Welt gibt es die Aussage Everything is a le! (Alles ist eine Datei), was meint diese Aussage und wie verhält sich dies für tatsächliche Dateien und wie für Verzeichnisse?

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

**c)** Beschreibe in je einem Satz, was diese fünf wichtigen Syscalls tun:
open, read, write, close, fork

Vorwissen:

- Systemcalls (Syscalls) sind Methoden, die bereitgestellte Funktionalitäten in dem Betriebsystem aufrufen. 
- Dabei wird die Kontrolle vom Programm an den Kernel übergeben.
- Was ist ein Filedescriptor:
	- Nach dem öffnen einer Datei, wird ein Filedescriptor zurückgegeben.
	- Dieser repräsentiert die geöffnete Datei
	- Der Inhalt der Datei ist dann in dem FD gespeichert
	- Jede geöffnete Datei bekommt einen FD
		- bei 10 geöffneten Datein wird man dann 10 FDen haben

**Lösung:**
- read: liest Bytes von einem Filedescriptor
- write: schreibt Bytes in einen Filedescriptor
- open: öffnet eine Datei und gibt einen Filedescriptor zurück
- close: schlieÿt einen Filedescriptor
- fork: erzeugt einen neuen Prozess als Kopie des aufrufenden

Allgemein zur Fork:
- Fork erzeugt einen Kopieprozess aus einem Vaterprozess, beide Prozess sind zunächst identisch. Die Kopie wird Kindprozess genannt und originale Prozess ist dann Vaterprozess.
- Wir wissen aus der Vorlesung, dass jeder Prozess einen eindeutigen PID enthält, damit können wir auch zwischen Vater- und Kindprozess, wichtig ist nun, der Vater behält seine PID aber das Kindprozess bekommt 0 als PID. (Dadurch kann man mit einer IF-Bedingung, Kind bzw. Vater spezifischer Code schreiben).
(Im Übungsblatt findet ihr mehr dazu (: )

---

### Aufgabe 1.2: Prozesszustände

**a)** Warum gibt es neben den Zuständen ready und running einen weiteren Zustand waiting? Was wird durch diesen Zustand erreicht?

Vorwissen: 
![[Pasted image 20230424195041.png]]
- Ausführliche Erklärung dazu findet ihr Im Kapitel 2 Folien 19-20 (II-19 / 20)

**Lösung:**
- Durch "waiting" wird eine bessere Auslastung der CPU ermöglicht
- Angnommen wir hätten den Zustand "waiting" nicht:
	- Wenn ein Prozess auf ein I/O-Ereignis warten würde, dann wird er in ready bleiben, da er nirgendswo gehen kann, obwohl er sogar keine Rechenzeit fordert
	- Der Prozess kann Busy Waiting machen, also er wird in die CPU bleiben und auf das I/O-Ereignis warte, wodurch die CPU nicht für andere Prozesse benutzt werden kann
	- Alternativ kann der Prozess wieder in den Zustand ready gehen, wobei er jedes mal bis sein Ereignis tritt, wieder hinten in der Schlage gereiht wird. Diese ist für die CPU sehr belastend, da durchgehend ein Kontextwechsel stattfinden würde.
- Also ist der Zustand "waiting" unverzichtbar, es reichen also Interrputs und Multiprogramming nicht, es muss eine extra Warteschlange erzeugt werden

---

**b)** Mit Hilfe des Programms top können Sie sich in der Linux-Shell anzeigen lassen, welche Prozesse aktuell vom Kernel verwaltet werden. Es bietet eine Echtzeit-Ansicht, d.h. es werden auch aktuelle Informationen zum Prozesszustand, zur CPU- und zur Speichernutzung angegeben. Auf einem normalen Linux-System werden Sie feststellen, dass sich eine große Zahl von Prozessen im Zustand 'sleeping' (entspricht nach unserer Definition waiting) befindet. Dies repräsentiert nicht den Zustand ready - dieser wird bei top mit dem Zustand running zusammengefasst. Was vermuten
Sie: warum befinden sich so viele Prozesse im Zustand waiting?

**Lösung:**
- Sind hilfsfunktionen die bei Bedarf aktiviert werden können
- Die Erzeugung folgt vorher, damit sie auch schnell ausgeführt werden können
- Sie sind alle in "sleeping" und nicht in "ready" damit sie z.B. durch Scheduling nicht ausgeführt werden, also in den Running-Zustand.
	- Dadurch wird unnötige CPU-Belastung stattfinden (Kontextwechsel)
- Sie werden aufgeweckt, wenn sie nötig sind.

---

### Aufgabe 1.3: Prozesse und Prozesserzeugung

**a)** In dieser Aufgabe sollen Sie ein C-Programm schreiben, welches eine globale Variable verwendet. Diese Variable soll am Anfang mit dem Wert 1 initialisiert und mittels einer Schleife bis zum Wert 50 (inklusive) inkrementiert werden. Des Weiteren soll das Programm folgende Funktionalität
implementieren:
- Innerhalb der Schleife soll eine Wartezeit eingefügt werden. Solange der Wert der globalen Variable kleiner oder gleich 25 ist, soll diese Wartezeit 300 Millisekunden betragen. Danach soll eine variable Zeit zwischen 100 und 500 Millisekunden gewartet werden. Benutzen Sie Zufallszahlen, um in jedem Schleifendurchlauf eine Wartezeit zu ermitteln. Verwenden Sie srand und rand zur Initialisierung und Erzeugung der Zufallszahlen.
- Erreicht die globale Variable den Wert 9, soll ein Kindprozess erzeugt werden.
- Nach dem Erstellen des Kindprozesses soll der Anfangswert für den Zufallszahlengenerator mit dem Befehl srand(time(NULL) +pid) reinitialisiert werden, wobei pid der Rückgabewert der Funktion fork sein soll.
- In jedem Schleifendurchlauf sollen Vater- und Kindprozess den Wert der globalen Variable ausgeben.

Erklären Sie die Ausgabe des Programms sowie die durch das Erstellen eines Kindprozesses aufgetretenen Effekte. Wieso hat man nach dem Erstellen des Kindprozesses den Anfangswert für den Zufallszahlengenerator neu initialisiert?

**Lösung: **

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <errno.h>
#include <signal.h>
#include <time.h>

#define TRUE 1
#define FALSE 0

int globalvariable;

int main() {

  pid_t pid = getpid();
  for (globalvariable = 1; globalvariable <= 50; globalvariable++)
  {
    if (globalvariable == 9) {
      pid = fork();
      srand(time(NULL)+pid);
    }
    if (pid == 0) { 
      printf("%d\n", globalvariable);
    }
    else { 
      printf("%d\n", globalvariable);
    }
    if (globalvariable <= 25)
    {
      usleep(300 * 1000);
    } else {
      usleep( (rand()%401 + 100) * 1000);
    }
  } 

  return 0;
}

```

- Der Vater druckt bei jedem Durchlauf die globale Variabele. 
- Wenn die globale Variabele den Wert 9 erreicht, wird ein Kindprozess erzeugt. (Kopie vom Vaterprozess)
- Kind- und Vaterprozess drucken dann die globale Variabele "doppelt"
- Nach dem Erstellen von dem Kindprozess bekommt er seinen eigenen Zufallszahlengenerator, damit nicht beide Prozesse den gleichen Zufallswert ziehen
- Wenn die globale Variable den Wert 26 erreicht, werden die Ausgaben von Vater und Sohn nicht mehr alternierend, da die Wartezeit nicht mehr für beide Konstant ist, sondern ein einem Interval liegt (100ms bis 500ms).

---

**b)** Erweitern Sie das Programm aus a) mit folgender Funktionalität:
 - Die Prozesse sollen beim Ausdrucken der Variable auch angeben, wer die Ausgabe vornimmt: "Kind: 15", "Vater: 16", ...
 - Ist ein Prozess fertig, meldet er sich zu Wort mit "Vater: Ich bin fertig!" oder "Kind: Ich bin fertig!"
 - Ist der Vater zuerst fertig, dann wartet er, bis sein Kind fertig ist, und beendet erst dann seine Ausführung. Belegen Sie die Korrektheit ihres Programms mit entsprechenden Ausgaben _(c.3)_.
 - Modifizieren Sie nun die vorangehende Funktionalität: Ist der Vater zuerst fertig, soll er nicht mehr auf sein Kind warten, sondern es stattdessen "töten". Das Kind soll sich anschließend vorzeitig mit einem Abschied beenden _(c.4)_.

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <errno.h>
#include <signal.h>
#include <time.h>

#define TRUE 1
#define FALSE 0

void stirb(int signal) {
  printf("Sohn: Ich wurde getoetet!\n");
  printf("Sohn: Ende!\n");
  exit(-1);
}

int globalvariable;

int main() {

  (void)signal(SIGTERM, stirb); 

  pid_t pid = getpid();
  for (globalvariable = 1; globalvariable <= 50; globalvariable++)
  {
    if (globalvariable == 9) {
      pid = fork();
      srand(time(NULL)+pid);
    }
    if (pid == 0) { 
      printf("Sohn: %d\n", globalvariable);
    }
    else { 
      printf("Vater: %d\n", globalvariable);
    }
    if (globalvariable <= 25)
    {
      usleep(300*1000);
    } else {
      usleep( (rand()%400 + 100) *1000);
    }
  } 

  if (pid == 0) {
    printf("Sohn: Ich bin fertig! Ende.\n");
  } else {
#if 1
    // c.3 
    printf("Vater: Ich bin fertig! Warte auf Sohn...\n");
    waitpid(pid, NULL, 0); 
#else
    if (waitpid(pid, NULL, WNOHANG) == 0) {
        // c.4
        printf("Vater: Amen fuer den Sohn...\n");
        kill(pid, SIGTERM);
    }
#endif
    printf("Vater: Ende.\n");
  } 

  return 0;
}
```
- Außer das Beenden ist der Durchlauf dieses Programms analog zur a)
- Prozesssynchronisation wird durch "wait" realisiert
- c.3: Hier wartet der Vaterprozess auf den Sohn
	- Wenn der fertig ist dann führt er "waitpid", dadurch wartet er auf ein Signal von dem Sohn, damit er weitermachen darf.
- c.4: Hier wartet der Vaterprozess nicht
	- Zunächst prüft er mit der If-Bedingung ob er vor dem Sohn fertig ist, wenn nicht dann überspringt er und beendet
	- Mit "WNOHANG", will der Vater zwar informiert werden wann der Sohn zuende gelaufen ist, wird aber dadurch nicht blockert wie bei c.3. 
	- Sonst, betet der Vater für seinen Sohn und führt die "kill" Funktion durch.
		- Der Sohnprozess bekommt den Signal und führt entsprechend die "stirb" Funktion. 
		- Am Ende terminiert er mit dem Wert -1 (entsprechend ist dies ein unerfolgreicher Ausführung also Error!)