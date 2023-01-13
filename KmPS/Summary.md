## Claßen Teil

### Concurrency
- bezeichnet die Möglichkeit, Programmteile unabhängig voneinander in unterschiedlicher Reihenfolge auszuführen zu können
- bei __mehreren__ Prozessoren oder Rechnern ist die parallele Ausführung möglich
- bei __nur einem__ Prozessor ist nur eine sequentialisierte Ausführung möglich, Reihenfolge variabel
- aber die Programmteile arbeiten gemeinsam an einer Aufgabe -> Abhängigkeiten
- Abhängigkeiten werden gelöst in dem:
	1. mehrere Prozessoren/Rechner: Die Ausführung einiger Teile muss warten oder mindestens pausiert werden
	2. ein Prozessor/Rechner: Kontrollfluss muss wechseln, damit der Sender seine Aufgabe erledigen kann

### Parallelismus
- bezeichnet die gleichzeitige Ausführung nebenläufiger Programmt