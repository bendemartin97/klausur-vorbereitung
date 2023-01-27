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
- führt dazu, dass das Go Programm nicht unbedingt blockiert, wenn eine Goroutine die Kontrolle nicht zurückgibt: Wenn es noch andere Threads gibt, in denen nicht blockierte Goroutinen sind.

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
	- ggf. sehr viele Goroutine und wenig Threads
	-  blockierende Goroutine blockieren nicht unbedingt den Thread. Es kommt einfach eine andere Goroutine auf dem Thread dran
- Goroutine werden von den sogenannten Green Threads verwaltet:
	- Scheduling durch Go Runtime und nicht durch BS-> BS unabhängig
	- alles passiert im user space von Go Runtime und nicht im kernel space
	- funktioniert auch auf BS ohne Thread Support
- Goroutine geben die kontrolle kooperativ ab, wenn idle oder blockiert oder an besonderen Stellen im Programmfluss oder explizite Abgabe mittels runtime.GoSched
- wenn eine Goroutine zu lange läuft, wird von dem Scheduler unterbrochen

### Channels
- Kommunikationskanäle, an der sich Goroutine anschließen können
- der Typ der zu übertagenden Werte muss bei der Erzeugung der Channel angegeben werden
- 2 Operationen: Send und Receive

### Unbuffered Channel
- können nur einen Wert transportieren, aber nicht halten
- wesentliche Technik zur Synchronization
- Receiver meldet Empfangwunsch an und wird blockiert -> Sender sendet -> Receiver kann empfangen
- Nachricht muss immer überreicht werden

### Buffered Channel
- können mehreren Wert transportieren und speichern
- wenn der Buffer voll ist, wird der Sender solange blockiert, bis der Buffer nicht mehr voll ist, sonst könnte der Sender nicht nicht-blockierend senden
- nicht zur Synchronization verwendbar

### Select Befehl - Receive
- select blockiert, bis auf einem der Channels eine Nachricht empfangen werden kann
- auch mit timeout realisierbar, damit der select nicht zu lange blockiert
- auch mit default realisierbar, damit der select auf einem Channel gar nicht blockiert

### Select Befehl - Send
- select blockiert, bis auf einem der Channels eine Nachricht gesendet werden kann
- auch mit timeout realisierbar, damit der select nicht zu lange blockiert
- auch mit default realisierbar, um zu vermeiden, dass select auf einem Channel blockiert, auf dem noch kein Receiver lauscht

### Aktorenmodell
- ein Modell in Informatik für nebenläufige Berechnungen und Programme
- diese werden in nebenläufigen Einheiten unterteilt, die ausschließlich über Nachrichtenaustausch kommunizieren

### Aktor
- sind nebenläufige Einheite, die nicht über einen gemeinsamen Speicherbereich verfügen, sondern ausschließlich über Nachrichten kommunizieren
- jede Aktor verfügt über ein Posteingang, eine Adresse und ein Verhalten
- der Empfang einer Nachricht wird als Ereignis bezeichet und diese werden im Posteingang gespeichert und nach FIFO Prinzip abgearbeitet
- das Verhalten eines Aktors beschriebt die Reaktion auf einer Nachricht abhängig von deren Aufbau
- Reaktionen:
	- Nachricht an sich selbst oder an einem anderen Aktor versenden
	- neue Aktore erzeugen
	- eigenes Verhalten ändern
- Der Nachrichtenaustausch erfolgt asynchron, d.h der Sender kann nach Absenden einer Nachricht direkt mit einer anderen Aktion fortsetzten, und muss darauf warten, dass der Empfänger die Nachricht akzeptiert
- Das Aktor Model beschreibt nicht, wie lange die Vermittlung einer Nachricht dauern darf, es ist nur definiert, dass jede Nachricht nach einer endlichen Zeit bei dem Empfänger ankommen muss.

### Nebenläufigkeit in Erlang
- leichtgewichtigte Erlang Prozesse -> OS abhängig
- entkoppelte parallele Prozesse, kein Shared Data, keine Seiteneffekte
- die Verteilung von Erlang Code auf mehreren Rechner leicht möglich
- nur der Erzeuger kennt den Prozess und dessen Prozess-ID eigentlich. Durch das globale registry können Client die Server-ID:s herausfinden. Registierung unter Alias. Konventi