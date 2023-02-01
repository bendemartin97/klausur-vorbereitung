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
- bedeutet, wenn das Ergebnis der Programmausführung von zeitlichen Verhalten bzw. von zeitlichen Abfolge bestimmter Programmteile abhängt, und diese zeitliche Abfolge nicht von dem Programm selbst eindeutig bestimmt werden kann.

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
- leichtgewichtigte Erlang Prozesse
- entkoppelte parallele Prozesse, kein Shared Data, keine Seiteneffekte
- die Verteilung von Erlang Code auf mehreren Rechner leicht möglich
- jede Server wird in einem Modul gepackt und mittels spawn ein Prozess erzeugt und gestartet. 
- Unterscheidung mehrerer Server:
	- Server schickt seine ID mittels self() mit. Client macht Patter Matching und matcht nur die Nachrichten, die von dem selbst kontaktierten Server stammen
	- Durch die Sendung der ID:s in beide Richtungen können viele Server parallel laufen und viele Client parallel bedient werden über das Netzwerk

### Erlang Prozesse
- werden vom dem Erlang VM erzeugt und verwaltet -> OS abhängig
- wenig Speicherbedarf, schnelle Erzeugung, sehr schnelle Kontextswitch
- skaliert gut, auch bei parallelen Rechnern
- sehr fehlertolerant
- nur der Erzeuger kennt den Prozess und dessen Prozess-ID eigentlich. Durch das globale registry können Client die Server-ID:s herausfinden. Registierung unter Alias. Konvention: Benutze den Modulnamen als Alias

### Erlang Nodes
- sind Erlang Laufzeitumgebungen
- auf einem Host können mehrere gleichzeitig laufen
- können aber auch auf mehreren durch Netzwerk (Internet) verbundenen Host verteilt werden
- werden über ihren Namen identifiziert

### Erlang Cookies
- gruppieren zusammengehörige Erlang Nodes
- Nodes können connecten, wenn der Cookie übereinstimmt
- auf gleichem Host haben alle Nodes den identischen Cookie

### Fehlerbehandlung in Erlang
- Let it crash
- Best Practice:
	- die Anwendung in zwei Teile aufteilen: einer, der die Aufgaben löst, und einer der die Errors behebt
	- der Teil, der die Aufgaben löst, soll mit so wenig defensivem Code, wie möglich geschrieben werden
	- der Teil, der die Errors behebt ist oft generisch, um die gleiche Fehlermeldung bei mehreren Anwendung verwenden zu können.
	- Diese führen zu einer sauberen Trennung der Themen und der Verringerung des Codevolumes
- bei parallelen Prozessen:
	- Verknüpfung von Prozessen, die auf der gleiche Aufgabe arbeiten mittels Links
	- bidirektionale Verbindung
	- wenn ein Prozess stirbt, sterben alle gelinkte Prozesse auch
- System Prozess:
	- begrenzt die Fehlerpropagation
	- wenn ein Prozess stirbt, werden benachrichtigt, sterben aber nicht
- Monitoring von Prozessen durch anderen Prozessen:
	- falls die ge-monitorte stirbt, wird der Monitoring Prozess benachrichtigt, aber nicht umgekehrt
	- unidirektional
	- der Monitoring Prozess muss nicht System Prozess werden

### Supervisor
- ist verantworlich ihre Kindprozesse zu starten, zu stoppen und zu monitoren
- kann Workers und / oder Supervisors überwachen, daruch entsteht ein Baum Struktur
- die Supervisor-Strategie gibt an, wie es in bestimmten Stellen im Baum bezüglich der Restart-Strategie zu verfahren ist, wenn ein Kind Prozess stirbt

### Worker
- sind für die normalen Berechnungen zuständing, ohne aufwändige Fehlerbehandlung

### Restart-Strategien:
- one-for-one
	- bedeutet wenn ein Supervisor viele Workers überwacht und eine von ihnen ausfällt, nur dieser eine neu gestartet werden muss
	- bei unabhängigen Prozessen zu verwenden
	- der Prozess kann seinen Zustand verlieren, ohne dass ihre Geschwisterprozesse davon betroffen wären
- one-for-all
	- ist immer dann anzuwenden, wenn alle Prozesse unter einem Supervisor starkt voneinander abhängen, um normal funktionieren zu können
- rest-for-all
	- bedeutet, wenn ein Prozess stirbt, werden alle nach ihm gestarteten (also abhängigen) Prozesse auch neu gestartet
- simple-one-for-one
	- ein Supervisor sitzt einfach da und weißt, dass er nur eine Art von Kindprozesse erzeugen kann
	- wenn man ein Kindprozess braucht, fragt man danach und bekommt es

### Event driven / Reactive Programming
- Laufzeitumgebung des Programms triggert Events
- diese werden im Programmcallback registriert, und bei entsprechenden Events aufgerufen
- JS Runtime ist immer single-threaded -> ein Thread mit einem Event Loop:
	- vermeide komplexe Anforderungen an die Plattform
	- JS Runtime ist ein großer Sichtbarkeitsbereich -> Seiteneffekte
	- die paralle Event Loops könnten wegen der Seiteneffekte die Semantik der Programm verändern
	- so wird jede Nachricht vollständig abgearbeitet
- ScriptJobs Queue:
	- vollständige Ausführung einer JS Script oder Modul
	- wenn ein Job im Queue ist, führe den ersten Job aus
	- zurück zum Event Loop
	- Script oder Modul muss vollstandändig abgearbeitet werden, bevor der Browser wieder rendern kann
- Nachteile:
	- langsame I/O Operationen blockieren die Ausführung
	- während einer Nachricht kann keine andere Nachricht abgearbeitet werden

### Callback Programmierstil
- asynchrone Programmteile mittels Callbacks und synchrone Funktionsaufrufe dürfen bei Abhängigkeiten voneinander nicht gemischt werden
- sonst werden sequentliell später kommenden Teile ggf. fehlerhafterweise vor dem sequentiell vorher kommenden, aber asynchrone programmierte Programmteile ausgeführt
- ebenso dürfen asynchrone Codeteile per Callbacks, die voneinander abhängen, einfach sequentiell programmiert werden, sonst ist die Ausführungsreihenfolge unklar. Die Verschachtelung der Callbackk Aufrufe notwendig

### Callback Hell
- Programmierung der Nebenläufigkeit der asynchronen Programmteile mittel Callbacks führt bei nacheinander auszuführenden (also abhängigen) Programmteile, zu einem tief verschachtelten und unübersichtlichen Programmcode

### Promise
- bezeichet ein Platzhalter-Objekt für ein Berechnungsergebnis, wobei die Berechnung ggf. noch nicht beendet ist
- dieses Platzhalter-Objekt kann von dem folgenden, von dem Wert abhängigen Programmteil direkt entgegengenommen werden, ohne dass die Berechnung berechnet sein muss
- wenn die asynchrone Programmteil beendet ist, kann der von dem Promise abhängigen Programmteil zur Ausführung kommen
- Zustand ist pendig, wenn die Berechnung noch nicht fertig berechnet wurde
- Ansonsten settled: entweder rejected oder resolved

### Executor
- ist eine Funktion mit im Promise-Standard festgelegten Signature und stellt den Parameter des Promise-Konstruktors dar. Sie wird sofort ausgeführt
- hat 2 Parameter:
	- sind Funktionen
	- eine, die den Wert als Parameter nimmt und diesen als positives
	- und eine, die den Wert als Fehlerergebnis in das Promise-Objekt einträgt
- nur der Rumpf des Executors muss vom Programmieren programmiert werden, inkl. mit der dortigen Aufruf von resolve und reject
- resolve und reject sind Callback Funktionen
- warum gibt diese 2 Parameter überhaupt:
	- um die Ergebnisse nicht über Seiteneffekte zu erzielen
	- die Werte sind in Executor eingeschlossen und können von der Außenwelt nicht mehr geändert werden
- resolve und reject ändern den Zustand der Promise auf settled, d.h weiter resolve und reject werden ignoriert

### .then, .catch und .finally
- .then für den daten-abhängigen Nachfolgecode
- .catch für Fehler-Callback
- Promise wird immer in seinem konrekten Wert resolved, und dann wird die Callback in .then ausgeführt
- .then:
	- wird immer verzögert ausgeführt, auch wenn das Promise Objekt schon den Wert hat
	- zuert ScriptJobsQueue abarbeiten: hängt weitere .then und .catch an dem PromiseQueue an
	- PromiseQueue abarbeiten
	- erst dann zurück zum EventLoop
	- .then gibt immer ein Promise Objekt zurück, können also verkettet werden
- catch für die einheitliche Fehlerbehandlung:
	- egal ob durch reject oder durch ein Exception in .then
- finally für Cleanup-Aktionen:
	- hat kein Parameter
	- gibt immer den Resultat oder den Fehler zurück
	- weiter Post-Cleanup-Aktionen möglich

### Promise.all
- wartet auf das Settlement mehrere Prommise
- bei asynchrone Plattform-Operation können parallel ausgeführt werden -> Perfomance Verbesserung
- wenn eine Promise rejected ist, werden alle Ergebnisse verworfen

### Promise.allSettled
- sammelt die Ergebnisse und Fehler in einem Array
- wenn eine Promise rejected ist, werden die Ergenisse nicht verworfen

### Promise.race
- überwacht das Settlement mehrerer Promises
- sammelt das Ergebnis von dem zuerst fertig werdenen Promise ein
- danach werden alle andere Ergebnisse verworfen

### Promisification
- bezeichnet die Umwandlung einer Library mit API Funktionen in Callback-Style in einer Variante in Promise-Style
- bei einer systematischen API der Library kann die Promisification der Library sehr systematisch gemacht werden, mittels einer generellen Wrapper Funktion promisify()
- Callback-Stlye:
	- wenn asynchrone Operationen mehrfach verwendet werden, und die Callbacks bei jedem Aufruf eine andere Wert liefern
- Promise-Style
	- wenn asynchrone Operationen nur einmal verwendet werden, an einer Stelle im Kontrollfluss
	- kann nur einen Resultatwert haben

### async / await
- asynchrone Funktionen
- im Rump kann mittels await auf die Resultatwerte anderer Funktionen gewartet werden
- async:
	- gibt immer eine Promise zurück
	- ist also new Promise(...)
	- ihr Rump ist also die Executor der neuen Promise
	- await wartet auf das Settlemant der Promise, d.h die Zeilen hinter einem await sind die Callback-Funktion eines .then
	- ihr Rückgabewert wird der resolved Wert der Promise
- await:
	- ermöglicht den daten-abhängigen Programmteil sequentiell hinter einem await zu implementieren, statt in Callback-Stil als Parameter eines .then
	- gibt den Kontrollfluss ab und wird getriggert, wenn die Promise in ihrem konrekten Wert resolved wird
- Problematiken:
	- await kann nur mit async oder auf top-level von Modulen verwendet werden
	- viele asnyc und await in Schleifen verlangsamen den Programmcode
	- await ist asynchron -> in await-Pausen können die globale Variablen geändert haben, da die Kontrolle abgegeben wurde

### Prototype
- jede JS Funktion hat automatisch ein Attribute prototype
- beim Erzeugen eines Funktions-Objekt wird das prorotype Attribut mit einem leerem Objekt initializiert
- beim Anlegen einer Funktion wird in dem automatisch angelegten Prototype Objekt ein Attribut constructor angelegt, welches auf das Funktions-Objekt zurückweist

### Prototypische Vererbung
- jedes Objekt kann ein anderes Objekt als Prototypen haben
- beim Lesen eines Objekts wird erst im Objekt selbst gesucht, danach die Prototype-Verweiskette gefolgt
- mehrere Objekte können den gleichen Prototype haben, der dann zentrale Methoden für alle diese Objekte implementieren kann
- neue Objekte können mittels new oder Object.create() initializiert werden
- new muss immer mit einer Funktion zusammen aufgerufen werden, welche die Rolle eines Konstruktor zur Objekt-Initializierung übernimmt

### Polyfilling
- Polyfilling bezieht sich auf die Praxis, ältere oder nicht vollständig unterstützte JavaScript-Funktionalitäten in älteren Browsern durch Code-Implementierungen zu ersetzen, die dieselbe Funktionalität bereitstellen. Dieser Code wird normalerweise als polyfill bezeichnet. Ein Beispiel dafür ist die Verwendung von Object.create()

### Tail-rekursion
- Tail-Rekursion ist eine Form der Rekursion, bei der der rekursive Aufruf die letzte von der Funktion ausgeführte Operation ist
- hat immer einen konstanten Platzaufwand, weil durch die Tail-Rekursion immer der gleiche Stack-Frame verwendet werden kann

### Call-By-Value
- werden die Parameter eines Funktionsaufrufs leftmost-innermost ausgewertet 
- nach vollständiger Auswertung der Parameter wird der Aufruf durch die rechte Seite der Funktionsdefinition ersetzt 
- die formalen Parameter werden durch die ausgewerteten Werte ersetzt

### Call-By-Name
- wird der Funktionsaufruf durch die rechte Seite ersetzt 
- die formalen Parameter werden durch die Ausdrücke (noch nicht ausgewertet) in den aktuellen Parametern ersetzt. 
- die dabei erhaltenen Ausdrücke werden dann leftmost-outermost ausgewertet

### Currying
- eine Funktion, die mehrere Parameter nimmt
- wird in eine Kette von Funktionen umgewandelt, die jeweils nur ein Argument entgegennimmt
- jede Funktion gibt eine neue Funktion zurück, die das nächste Argument erwartet, bis alle Argumente verarbeitet wurden und das engültige Ergebnis zurückgegeben wird

### Lazy Evaluation
- eine Funktion, die es erlaubt, bestimmte **Ausdrücke nur dann auszuwerten, wenn ihre Werte benötigt werden.**
- die Leistungsverbessurung und Reduzierung der Speichernutzung, indem unnötige Berechnungen vermieden werden.