## Claßen Teil

### Concurrency
- bezeichnet die Möglichkeit, Programmteile unabhängig voneinander in unterschiedlicher Reihenfolge auszuführen zu können
- bei __mehreren__ Prozessoren oder Rechnern ist die parallele Ausführung möglich
- bei __nur einem__ Prozessor ist nur eine sequentialisierte Ausführung möglich, Reihenfolge variabel
- aber die Programmteile arbeiten gemeinsam an einer Aufgabe -> Abhängigkeiten
- Abhängigkeiten werden gelöst in dem:
	- mehrere Prozessoren/Rechner: Die Ausführung einiger Teile müssen warten oder ggf. pausiert werden
	2. ein Prozessor/Rechner: Kontrollfluss muss wechseln, damit der Sender seine Aufgabe erledigen kann
![[Bildschirm­foto 2023-01-13 um 19.41.00.png]]
### Parallelismus
- bezeichnet die gleichzeitige Ausführung nebenläufiger Programmteile auf mehreren Prozessoren oder Rechnern!
- Falls während der Ausführung Abhängigkeiten über Daten oder Kommunikation auftauchen, die Ausführung einiger Teile müssen warten, ggf. pausiert werden![[Bildschirm­foto 2023-01-13 um 19.41.10.png]]