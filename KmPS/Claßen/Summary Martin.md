### Concurrency
- bezeichnet die Möglichkeit mehrere Programmteile unabhängig voneinander in verschiedener Reihenfolge auszuführen
- bei mehreren Prozessoren oder Rechner: parallele Ausführung möglich
- bei einem Prozessor oder Rechner: sequentielle Ausführung möglich, Reihenfolge variable
- Die Programmteile arbeiten aber trotztdem zusammen -> Abhängigkeiten
- Abhängigkeiten werden gelöst, in dem:
	- bei mehreren Prozessor / Rechner: die Ausführung einiger Teile müssen warten ggf. pausiert werden
	- bei einem Prozessor / Rechner: das Kontrollfluss muss wechseln, damit der Sender seine Aufgaben erledigen kann

### Parallelismus
- bezeichnet die gleichzeitige Ausführung nebenläufiger Programmteile auf mehreren Prozessoren oder Rechnern
- falls während der Ausführung Abhängigkeiten über Daten oder Kommunikation auftauchen. muss die Ausführung einiger Teile warten ggf. pausiert werden

### Scheduler
- verteilt nebenläufige Programmteile aus zur Verfügung stehenden Ausführungseinheiten und bistimmt ggf. kontrolliert ihre Ausführung
- Der Scheduler:
	- sichert den Prozesskontext des gerade unterbrochenen Tasks
	- wählt den nächsten Prozess aus
	- stellt dessen Prozesskontext her
	- gibt den Prozessor an dem neuen Prozess ab

### Kooperatives Scheduling
- der Scheduler überlässt selbst jedem Prozess die Entscheidung, wann er die Kontrolle an dem Scheduler zurückgibt
- in der Regel sind jede Dienst-Anforderungen an dem BS mit einem Taskwechsel verbunden
- Vorteile:
	- hilt den Race Conditions zu vermeiden
- Nachteile:
	- kein schnelles Reagieren auf Ereignisse
	- Tasks, die die Kontrolle nicht schnell genug zurückgeben, können das gesamte System blockieren

### Preemptive Scheduling:
- bezeichnet die Scheduling-Strategie, bei der der Scheduler jeden Prozessen eine bestimmte Rechnerzeit zuteilt und dem Prozess den CP