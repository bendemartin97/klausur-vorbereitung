# Concurrency

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

# GO

### Goroutinen
- __werden auf die Nebenläufigkeitsmechanismen des Betriebssystems (Threads über CPU:s / Kernen) abgebildet__
- Unterschied zwischen Goroutinen und Threads:
	- aber viel leichtgewichtiger als Threads (Threads ca. 1 MB Stack size, Goroutine ca. 2 KB)
	- __Threads: preemptive Scheduling__, viele Register müssen gesichert werden, teuer Context Switch
	- __Goroutinen: kooperatives Scheduling im Go Runtime__, leichtgewichtiger Context Switch
- __die Goroutinen werden vom Go Runtime auf verfügbare Threads verteilt__, um möglichst viele von ihnen nebenläufig betreiben zu können:
	- ggfs. (sehr) viele Goroutinen, wenige Threads.
	- blockierende Goroutine blockiert nicht den Thread. Es kommt einfach eine andere Goroutine auf dem Thread dran.
- Verwaltung von Go Runtime: sogenannte Green Threads
	- __Scheduling durch das Go Runtime, nicht durch das OS__
	- alles passiert um user space des Go Runtime, nicht im kernel space
	- funktioniert auch auf OS:s ohne Thread Support
- meist keine explizite Abgabe der Kontrolle, sondern implizite Abgabe der Kontrolle z.B Aufruf von Libraryfunktionen
- __Goroutine gibt Kontrolle kooperativ ab__, wenn idle oder blockiert oder an besonderen Stellen im Programmablauf oder mittels explizite Abgabe  runtime.GoSched() (normal)
- __wenn eine Goroutine zu lange läuft, wird von Scheduler unterbrochen__
#### Tight Loop
- bezeichnet eine Goroutine, __die eine lange laufende Schleife oder Endlosschleife ausführt und dabei die Kontrolle nicht abgibt__ , da sowohl keine Library Funktionen aufgerufen werden (welche die Kontrolle dann abgeben würden) als auch kein runtime.Gosched() innerhalb der Schleife ausgeführt wird, welches die Kontrolle explizit abgeben würde. D.h. in der Schleife werden nur "Elementaroperationen", z.B. elementare Berechnungen, ausgeführt.![[Bildschirm­foto 2023-01-14 um 09.22.03.png]]

### Channels
- sind __Kommunikationskanäle, an die sich die Goroutinen anschließen können__
- Datentyp der zu übertragenden Werte bei Erzeugung angeben
- Operationen: Senden und Empfange

#### Unbuffered Channels
- __default, d.h er kann nur einen Wert transportieren, aber nicht speichern__
- send blockiert, wenn sich noch kein Receiver empfangsbereit ist, receive blockiert, bis ein Sender einen Wert sendet
- blockieren innerhalb einer Goroutine: andere Goroutine wird gescheduled
- blockieren im Hauptprogramm führt dazu, dass dieses wartet, d.h eine andere Goroutine gescheduled wird
- __wesentliche Technik zur Synchronitazation__
- Empfänger wird durch receive blockiert, bis der Sender per send mitteilt, dass er den Sync-Point erreicht hat
- __Nachricht muss immer überreicht werden__ -> Sender und Receiver immer minimal blockiert
- Receiver meldet Empfangswunsch an und wird blockiert -> Sender sendet -> Receiver kann empfangen
- Verwendung:
	- synchronisierten Beenden von main: wird oftmals verwendet, um das Hauptprogramm main() zu informieren, dass auch die letzte weitere Goroutine beendet ist und main() sich somit beenden kann, ohne dass dadurch eine Goroutine abgebrochen wird.
	- Erreichbarkeit von Webseiten prüfen

#### Buffered Channels
- __können Nachrichten halten und transportieren__
- wenn Buffer voll ist, werden die send Operationen blockert, bis Buffer nicht mehr voll ist, sonst können Sender nicht-blockierend senden
- nicht zur Synchronisation verwendbar

### select Befehl
#### Receive:
- select wartet (blockiert), bis auf einem der Channels eine Nachricht eintrifft. case für die einzelnen Fälle.
- auch mit Timeout realisierbar, damit das select nicht zu lange blockiert
- auch mit default case realisierbar, damit ein receive auf einem Channel gar nicht blockiert
#### Send:
- select wartet (blockiert), bis auf einem der Channels eine Nachricht gesendet werden kann.
- auch mit Timeout realisierbar, damit das select nicht zu lange blockiert
- auch mit default case realisierbar, damit der Sender vermeidet, beim send auf einem Channel blockiert zu werden, falls noch kein Receiver an dem Channel lauscht

### WaitGroups
- __andere Möglichkeit drauf zu warten, dass eine bestimmte Anzahl von Goroutinen endet__
- der WaitGroup wird die Anzahl der Goroutinen mitgeteilt. Sie wartet dann bei Wait() auf die entsprechende Anzahl von Done() Meldungen.
- die erwartete Anzahl lässt sich auch dynamisch / nachträglich noch mittels Add() erhöhen.
- __waitGroup Objekte dürfen nicht kopiert werden__. D.h. Weitergabe als Funktionsparameter nur per Pointer.

### Timer
- werden gestartet und schicken bei Ablauf eine Nachricht mit dem Zeitstempel des Ablaufzeitpunkts auf den ihnen jeweils zugeordneten Channel.
- __gedacht für einmalige, zeitabhängige Aktionen.__
- Aähnlich wie ein time.Sleep() , aber:
	- Timer können vor Ablauf wieder gestoppt werden,
	- Timer-Setup ist nicht wie time.Sleep() synchron & blockierend, sondern per-se asynchron.

### Ticker
- werden gestartet und schicken dann wiederholt jeweils eine Nachricht mit dem Zeitstempelwert des jeweiligen Ticks auf einen ihnen jeweils zugeordneten Channel.
- __gedacht für wiederholt wiederkehrende zeitabhängige Aktionen.__

### Shared Data
- __in Go auch das Arbeiten mit Shared Data möglich.__ Also Prinzipien nicht so strikt wie z.B. beim Aktorenmodell von Erlang.
- Mechanismen:  
	- Mutexe: mutual exclusion bei kritischen Bereichen.
	- Atomic counters: spezielle Zählervariablen mit speziellen atomaren Zugriffsoperationen.
- Diese und andere Mechanismen sind auch mittels Channels „emulierbar“.

# Erlang
### Aktorenmodell
- ein Modell in Informatik für nebenläufige Berechnungen bzw. Programme
- diese werden in nebenläufige Einheiten (Aktoren) unterteilt, die __ausschließlich über Nachrichtenaustausch kommunizieren__
#### Aktoren:
- sind nebenläufige Einheiten, die nicht über einen geteilten Speicherbereich verfügen, sondern ausschließlich über Nachrichten kommunizieren
- die Kapselung des Zustandes des Aktors ähnelt dem Prinzip der Kapselung in der objektorientierten Programmierung.
- __Jeder Aktor verfügt über einen Posteingang, eine Adresse und ein Verhalten__![[Bildschirm­foto 2023-01-14 um 10.39.32.png]]
- der Empfang einer Nachricht wird als __Ereignis__ bezeichnet -> werden __in Posteingang gespeichert__ und werden nach FIFO-Prinzip bearbeitet
- Das Verhalten des Aktoren beschreibt Reaktionen auf Nachrichten abhängig von deren Aufbau
- Reaktionen:
	1. __Nachrichten an sich selbst__ oder __an andere Aktoren__ verschicken. 
	2. __neue Aktoren erzeugen.__ 
	3. das __eigene Verhalten ändern.__
- __Der Nachrichtenaustausch erfolgt asynchron__, d. h. der Sender einer Nachricht __kann nach dem Abschicken 
- sofort mit anderen Aktionen fortfahren__ und __muss nicht warten, bis der Empfänger die Nachricht akzeptiert.__
- Das Actor Model __legt nicht fest, wie lange die Vermittlung einer Nachricht dauern darf.__ Es wird nur definiert, dass __jede Nachricht nach endlicher Zeit beim Empfänger ankommen muss__.
- Aktoren __zu einem Zeitpunkt nur eine Nachricht abarbeiten__ und __kein anderer nebenläufiger Prozess den internen Speicherbereich__ eines Aktors __beinflussen darf__, führt dazu, dass __die Programmierung innerhalb des Aktors rein sequentiell ist__
- kein Flow of Control, Verhalten wird reihenfolgenlos festgelegt


### Nebenläufigkeit in Erlang
- leicht-gewichtige Erlang-Prozesse
- entkoppelte parallel Prozesse (kein Shared Data, keine Seiteneffekte)
- die Verteilung von Erlang Code über viele parallele Rechner leicht möglich
- jede Server wird in einem Modul gepackt und mittels spawn ein Prozess erzeugt und gestartet. Über die Prozess-ID werden dem Prozess Erlang-Nachrichten gesendet![[Bildschirm­foto 2023-01-14 um 11.24.37.png]]

### Erlang Prozesse
- werden in der Erlang VM erzeugt und verwaltet -> OS unabhängig
- wenig Speicherverbrauch, sehr schnelle Erzeugung, schneller Prozesswechsel
- skaliert gut, auch bei parallelen Rechnern
- sehr fehlertolerant: wenn man alles auf viele kleine Einzelprozesse aufteilt, crasht bei Softwarefehler nur einer dieser Prozesse, nicht das Gesamte.

