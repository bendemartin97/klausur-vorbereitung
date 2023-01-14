## Claßen Teil

### Concurrency
- bezeichnet die Möglichkeit, Programmteile unabhängig voneinander in unterschiedlicher Reihenfolge auszuführen zu können
- bei __mehreren__ Prozessoren oder Rechnern ist die parallele Ausführung möglich
- bei __nur einem__ Prozessor ist nur eine sequentialisierte Ausführung möglich, Reihenfolge variabel
- aber die Programmteile arbeiten gemeinsam an einer Aufgabe -> Abhängigkeiten
- Abhängigkeiten werden gelöst in dem:
	- mehrere Prozessoren/Rechner: Die Ausführung einiger Teile müssen warten oder ggf. pausiert werden
	- ein Prozessor/Rechner: Kontrollfluss muss wechseln, damit der Sender seine Aufgabe erledigen kann![[Bildschirm­foto 2023-01-13 um 19.41.00.png]]
	
### Parallelismus
- __bezeichnet die gleichzeitige Ausführung nebenläufiger Programmteile auf mehreren Prozessoren oder Rechnern__
- Falls während der Ausführung Abhängigkeiten über Daten oder Kommunikation auftauchen, die Ausführung einiger Teile müssen warten, ggf. pausiert werden![[Bildschirm­foto 2023-01-13 um 19.41.10.png]]

### Scheduler, Scheduling
- verteilt nebenläufige Programmteile / Tasks auf die zur Verfügung stehenden Ausführungseinheiten, wie z.B Prozessoren und kontrolliert bzw. bestimmt ggfs. ihre Ausführung
- Der Scheduler:
	- sichert den Prozesskontext des gerade unterbrochenen Tasks
	- wählt den nächsten Prozess aus
	- stellt dessen Prozesskontext her
	- gibt den Prozessor dann an diesen neuen Prozess ab
- Der Scheduler kann Listen mit verschieden priorisierten Tasks führen, und niedrig priorisierte entsprechend selten aufrufen.

### Kooperatives Scheduling (Phyton, JS, Go)
- __überlässt jedem Task selbst die Entscheidung, wann er die Kontrolle an den Scheduler zurückgibt__
- in der Regel wir zumindest jede Dienst-Anforderung an das Betriebssystem mit einem Taskwechsel verbunden
- Vorteil:
	- hilft Race Conditions zu vermeiden
- Nachteil:
	- kein schnelles Reagieren auf Ereignisse
	- Tasks, die die Kontrolle nicht schnell genug zurückgeben, können das gesamte System / andere Tasks (zeitweise) blockieren

### Mehrstufige Nebenläufigkeit in Go
- ein oder mehrere Prozessoren. 
- darüber mehrere Threads: Mindestens ein Thread pro Prozessor. Aber ggfs. auch mehrere Threads pro Prozessor. 
- darüber die Goroutinen.
- führt dazu, dass das Go Programm nicht unbedingt blockiert, wenn eine Goroutine die Kontrolle nicht zurückgibt: Wenn es noch andere Threads gibt, in denen nicht blockierte Goroutinen sind.

### Preemptive Scheduling (Java, Erland, Betriebssystemen)
- bezeichnet die Scheduling-Strategie, __bei welcher der Scheduler den Prozessen / Tasks eine Rechenzeit zuteilt und dem aktuellen Prozess die CPU nach Ablauf dieser Zeit wieder entzieht__, egal, an welcher Stelle der Prozess mit seinen Berechnungen gerade ist
- Vorteil:
	- der Flow of Control kann einem Task an beliebiger Stelle entzogen werden
	- schnelles Reagieren
- Nachteil:
	- Kombination mit Shared Data: das nächste Task ändert unbemerkt die gemeinsam genutzte Datenstruktur
	- es ist schwer per Locking alle gemeinsam genutzter Ressource effektiv zu schützen

### Deadlock
- bezeichnet einen zyklischen Wartezustand zwischen mehreren Prozessen / Tasks, wobei jeder Task auf eine bestimmte Aktion (Nachricht, Datenänderung, Freigabe eines Betriebsmittels wie Datei, ...) wartet und diese Aktion nur exklusiv von diesem anderen Task ausgeführt werden kann

### Race Condition
- bedeutet, dass das Ergebnis der Programmausführung vom zeitlichen Verhalten bzw. der zeitlichen Abfolge bestimmter Programmteile abhängt, wobei diese zeitliche Abfolge nicht eindeutig durch das Programm selbst bestimmt wird.

### Goroutinen
- meist keine explizite Abgabe der Kontrolle, sondern implizite Abgabe der Kontrolle z.B Aufruf von Libraryfunktionen
- explizite Abgabe mittel runtime.GoSched()