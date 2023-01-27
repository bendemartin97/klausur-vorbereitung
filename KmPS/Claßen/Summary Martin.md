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

### Mehrstufige Nebeläufigkeit in GO
- auf einem oder mehreren Prozessor
- darüber Threads
- darüber Goroutine
- führt dazu, dass Threads nicht unbedigt blockieren, wenn eine Goroutine die Kontrolle nicht schnell genug zurückggeben. Es kommt einfach eine andere Goroutine auf dem Thread dran

### Preemptive Scheduling
- bezeichnet die Scheduling-Strategie, bei der der Scheduler jeden Prozessen eine bestimmte Rechnerzeit zuteilt und dem Prozess den CPU nach der Ablauf dieser Zeit wieder entzieht, egal, an welcher Stelle der Prozess mit seiner Berechnung gerade ist
- Vorteile:
	- schnelles Reagieren
	- der Flow of Control kann an beliebigen Stellen entzogen werden
- Nachteile:
	- Kombination mit Shared Date: der nächste Task kann unbemerkt die gemeinsam genutzte Daten verändern
	- es ist schwer per locking alle gemeinsam genutze Ressourcen effektiv zu schützen

### Deadlock
- beschriebt einen zyklisen Wartezustand zwsichen mehreren Prozessen die auf einer bestimmte Aktion warten und diese Aktion nur exklusiv von diesem anderem Prozess ausgeführt werden kann

### Race Condition
- bedeutet, wenn das Ergebnis der Programmausführung von zeitlichen Verhalten bzw. von zeitlichen Abfolge bestimmter Programmteile abhängt, und diese zeitliche Abfolge nicht von dem Programm selbs eindeutig bestimmt werden kann.

### Goroutine
- werden auf die Nebenläufigkeitsmechanismen des Betriebssystem abgebildet (Threads)
- Unterschied zwischen Goroutine und Thread:
	- Goroutine sind leicht-gewichtiger, ein Thread hat 1 MB Stack size, eine Goroutine nur 2 KB
	- Threads verwenden preemptive Scheduling: viele Register müssen gesichert werden -> teuere Kontextswitch
	- Goroutine verwenden kooperative Scheduling in Goruntime: leicht-gewichtigere Kontextswitch
- Goroutine werden vom dem Go Runtime auf verfügbare Threads verteilt:
	- alles passiert im user space von Go Runtime und nicht im kernel space
	- 