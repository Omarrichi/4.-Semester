21.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

Tut mir leid für die Verspätung, da aber das Thema etwas komplex sein kann, und viele wegen DSAL Einsicht nicht da seien konnten, habe ich mich entscheiden die Notizen ausführlicher zu machen.


Dieses Blatt ist Teil 2 des Kapitels "Speicherverwaltung"

Zunächst **Allgemeines:**

Wenn ein Prozess ausgeführt wird, werden entsprechend seine Daten geladen dies geschieht indem die Seiten in den Hauptspeicher geladen werden. Jedoch hat der Hauptspeicher nicht genug Platz für alle diese Seiten für alle ausgeführten Prozesse, deswegen werden nur die nötigen Seiten in den Hauptspeicher geladen. Wenn ein Prozess neu ausgeführt wird dann müssen seine Seiten geladen werden, dies gewcshi

- Seitenfehler:
	- Wenn die MMU eine Seite im Hauptspeicher anfragt passiert folgendes:
	- Die Seite ist im Hauptspeicher, wird also an die CPU gegeben
	- Die Seite ist nicht im Speicher