**Donnerstag 2. Februar 2023, 8:30-10:30 Uhr.** 
**Dauer der Klausur: 120 Minuten**

**Wichtig!:**
Diese Präsentation umfasst nicht alle Themen des Semesters aus meinem Teil. Somit gibt es auch nicht zu allen Teilen einen „Beispiel-Fragenkatalog“. Die anderen Themen sind aber genauso prüfungsrelevant: Alle meine Themen aus dem Semester sind komplett prüfungsrelevant! Die Idee der Prüfungsinfos ist ja, Ihnen konkret ein Verständnis zu geben, wonach ich in schriftlichen bzw. mündlichen KMPS Prüfungen frage, d.h. wie sie sich für die Prüfungen vorbereiten müssen / sollten. Dies wird beispielhaft anhand ausgewählter Themen des Semesters konkret gemacht. Dieselbe „Idee der Art zu fragen“ gilt dann auch für alle meine anderen Themen des Semesters!

# Fragen/Aufgaben

## Go / Concurrency / Parallelismus

1. Beschreiben Sie, was man unter Concurrency und was man unter Parallelismus versteht. Grenzen Sie die beiden gegeneinander ab
	-  Concurrency beschreibt die Fähigkeit eines Systems, mehrere Aufgaben **gleichzeitig zu verwalten**. Es geht darum, dass verschiedene Aufgaben **parallel ausgeführt werden, aber nicht unbedingt gleichzeitig auf einem einzigen Prozessor.** Ein Beispiel für Concurrency ist das Verwalten von mehreren Anwendungen auf einem Computer, wobei jede Anwendung im Hintergrund läuft und die CPU-Zeit teilt.
	- Parallelismus beschreibt die Fähigkeit eines Systems, mehrere Aufgaben **gleichzeitig auf mehreren Prozessoren oder Kerne auszuführen**. Es geht darum, dass die Aufgaben **tatsächlich gleichzeitig auf unterschiedlichen Prozessoren** ausgeführt werden, um die Leistung zu steigern. Ein Beispiel für Parallelismus ist die Verwendung von mehreren Kerne in einem Computer, um die Ausführung einer Aufgabe zu beschleunigen.
	- Concurrency und Parallelismus sind eng miteinander verbunden, aber nicht dasselbe. Concurrency konzentriert sich darauf, wie Aufgaben verwaltet werden, während Parallelismus sich darauf konzentriert, wie Aufgaben ausgeführt werden. **Ein System kann concurrency haben, ohne parallelismus zu haben, aber es kann nicht parallelismus haben, ohne concurrency zu haben.**

2. Beschreiben Sie das Scheduling nebenläufiger Programmteile in Go und in Erlang und grenzen Sie diese beiden Scheduling Konzepte gegeneinander ab. 
	- Das Scheduling in Go und Erlang unterscheidet sich in der Art und Weise, wie sie mit Threads umgehen. **Go verwendet ein M:N-Modell**, bei dem eine feste Anzahl von Os-Threads verwendet wird, um eine dynamische Anzahl von Goroutinen auszuführen. **Erlang verwendet ein 1:1-Modell,** bei dem jeder Prozess einen Os-Thread verwendet. _Dies bedeutet, dass Erlang in der Lage sein kann, mehr Prozesse auf einer begrenzten Anzahl von Os-Threads auszuführen, während Go möglicherweise mehr Os-Threads verwendet, um die gleiche Anzahl von Goroutinen auszuführen._
	- Beide Scheduler haben ihre eigenen Vorteile und Nachteile. Erlang's 1:1-Modell ermöglicht es, mehr Prozesse auf einer begrenzten Anzahl von Os-Threads auszuführen und ermöglicht es Prozessen, schnell und effizient zu kommunizieren, während Go's M:N-Modell es ermöglicht, mehrere Kerne besser auszunutzen und es ermöglicht es Goroutinen, schnell und effizient zu kontextwechseln. 

3. Was versteht man unter kooperativem Scheduling? Nennen Sie dabei auch einen Vorteil und einen Nachteil dieses Scheduling Ansatzes. Beschreiben Sie das kooperative Scheduling in der Programmiersprache Go.
	- Kooperatives Scheduling bezieht sich auf eine Art von Scheduling, bei der die verwalteten Aufgaben, wie z.B. Threads, **aktiv daran mitwirken, den Zeitpunkt des Kontextwechsels zu bestimmen.** Im Gegensatz dazu wird bei **präemptiven** Scheduling der **Scheduler entscheiden, wann der Kontext einer Aufgabe gewechselt wird.**
	- Ein _Vorteil_ von kooperativem Scheduling ist, dass es die Programmierung einfacher macht, da die Entwickler die Kontrolle darüber haben, wann ein Kontextwechsel stattfindet. Dies ermöglicht es ihnen, ihre Anwendungen besser zu optimieren und potenzielle Probleme mit Deadlocks oder anderen Formen von Blockierungen zu vermeiden.
	- Ein _Nachteil_ von kooperativem Scheduling ist, dass es leicht zu Fehlern führen kann, wenn Entwickler die Kontrolle über den Kontextwechsel nicht richtig ausüben. Eine Aufgabe kann zum Beispiel in eine Endlosschleife geraten, ohne jemals den Kontext freizugeben, was zu Blockierungen und Leistungsproblemen führen kann.
	- Goroutinen sind ein Beispiel für kooperatives Scheduling. Goroutinen ermöglichen es Entwicklern, nebenläufige Aufgaben zu erstellen, indem sie das Schlüsselwort "go" verwenden. Goroutinen teilen sich einen gemeinsamen Stack und die Ausführung **findet kooperativ statt**, das heißt, dass eine **Goroutine dem Scheduler signalisiert, dass sie bereit ist, den Kontext freizugeben,** bevor sie blockiert oder einen anderen Zustand erreicht, in dem sie nicht weiter ausgeführt werden kann.

4. Wie wird der Wechsel der Kontrolle zwischen den nebenläufigen Goroutinen eines Go Programms veranlasst? Muss der Programmierer des Programms dies immer selbst programmieren?
	- wird durch den Go-Scheduler veranlasst.
	- **Der Programmierer muss jedoch darauf achten, dass er die Goroutinen in einer Weise schreibt, die es dem Scheduler ermöglicht, den Kontextwechsel durchzuführen.** Dies bedeutet, dass der Programmierer dafür sorgen sollte, dass die Goroutinen regelmäßig blockierende Operationen ausführen (z.B. lesen oder schreiben auf ein I/O-Gerät) oder dass sie sich selbst beenden, wenn sie ihre Arbeit abgeschlossen haben.
	- Ein Beispiel dafür ist das Verwenden von **channels**, um die Kommunikation zwischen Goroutinen zu koordinieren, bei dem eine Goroutine blockiert wird, wenn sie auf eine Nachricht wartet und dann wieder aufgenommen wird, wenn die Nachricht empfangen wird.
	- _Zusammenfassend,_ der Programmierer muss darauf achten, dass Goroutinen blockierende Operationen ausführen oder sich selbst beenden, um den Scheduler in die Lage zu versetzen den Kontextwechsel zu veranlassen, aber er muss nicht immer selbst die Kontrolle über den Kontextwechsel programmieren, da dies von Go Scheduler erledigt wird.

5. Beschreiben Sie die Konzepte der Programmiersprachen Go und Erlang betreffend geteilte Daten zwischen nebenläufigen Programmteilen.
	-   Go verwendet Goroutinen für nebenläufige Programmteile und teilt denselben Speicherbereich. Synchronisation von Zugriffen auf gemeinsam genutzte Daten erfolgt mittels Mutexes, Semaphoren und Channels.
	- Erlang verwendet Prozesse für nebenläufige Programmteile und jeder Prozess hat seinen eigenen Speicherbereich. Synchronisation erfolgt durch message passing und das Konzept der "nicht-gefährlichen paralleler Programmierung"
	- Go hat explizite Synchronisationsmechanismen, Erlang hat keine explizite Synchronisation. <small>(Zugriff auf die Daten wird automatisch synchronisieren, ohne dass es zu Fehlern oder inkonsistenten Zuständen kommt.)</small>
	- In Go gibt es die Möglichkeit zu Deadlocks und Kollisionen, Erlang hat keine Deadlocks oder Kollisionen.

6. Nennen Sie den wesentlichen Unterschied zwischen Goroutinen und den Threads des zugrundeliegenden Betriebssystems, der dazu führt, dass Goroutinen nicht 1:1 auf Threads abgebildet werden.


7. Was versteht man in Go unter einem Tight Loop Szenario? Geben Sie ein Beispiel (Go Codeausschnitt) dafür an. Wie kann der Programmierer dieses Problem lösen?
	- Ein Tight Loop Szenario in Go bezieht sich auf den Fall, in dem eine **Goroutine in einer Schleife (loop) hängt**, die sehr oft ausgeführt wird, ohne dass sie den Kontext freigibt. Dies kann dazu führen, dass die Goroutine die CPU-Zeit dominiert und andere Goroutinen nicht ausgeführt werden können, was zu Leistungsproblemen führen kann.
	- Der Programmierer kann das Problem eines Tight Loop Szenarios lösen, indem er die Goroutine anweist, den Kontext freizugeben, mit z.B. `runtime.Gosched()`. Oder man kann die Berechnungen in kleinere Blöcke aufteilen und diese mit einem channel oder WaitGroup synchronisieren.

8. Beschreiben Sie, wofür Channels in Go genutzt werden. Welche Arten von Channels gibt es? Für welche Kommunikationsszenarien werden diese genutzt? Mittels welcher Operationen können die Goroutinen solche Channels nutzen? 
	- Es gibt zwei Arten von Channels in Go: unidirektionale und bidirektionale Channels. Unidirektionale Channels ermöglichen es Goroutinen, nur Daten zu senden oder nur Daten zu empfangen. Bidirektionale Channels ermöglichen es Goroutinen, sowohl Daten zu senden als auch zu empfangen.
	- Channels werden in Go genutzt, um die Kommunikation und Synchronisation zwischen Goroutinen in verschiedenen Szenarien zu ermöglichen. Beispiele sind:
		- das Senden von Daten von einer Goroutine an eine andere Goroutine,
		- das Synchronisieren von Goroutinen, indem eine Goroutine auf eine Nachricht wartet, bevor sie fortfährt,
		- das Koordinieren von paralleler Verarbeitung, indem mehrere Goroutinen auf eine gemeinsam genutzte Datenquelle zugreifen.
	- Goroutinen können Channels nutzen, indem sie die Operationen "make", "send" und "receive" verwenden.
	- _Example_
		``` Go
		// create a new channel 
		dataChannel := make(chan int) 
		
		// Goroutine 1 
		go func() { 
			// send data on the channel 
			dataChannel <- 42 
		}() 
		
		// Goroutine 2 
		go func() { 
			// receive data from the channel 
			receivedData := <-dataChannel 
			fmt.Println("Received data:", receivedData) 
		}()
		```

	
9. Wie verhalten sich die grundlegenden send und receive Operationen bei Unbuffered Channels?
	- Bei Unbuffered Channels verhalten sich die grundlegenden send und receive Operationen wie folgt:
		- **Der send-Befehl blockiert, bis ein Empfänger bereit ist**, die Daten zu empfangen. Dies bedeutet, dass die send-Operation aufhört, ausgeführt zu werden, bis ein Empfänger bereit ist, die Daten zu empfangen.
		- **Der receive-Befehl blockiert, bis ein Sender Daten sendet**. Dies bedeutet, dass die receive-Operation aufhört, ausgeführt zu werden, bis ein Sender Daten sendet.
	- Dies ist das Verhalten, das als _"synchronized send and receive"_ bezeichnet wird, da sowohl Sender als auch Empfänger blockieren, bis die Kommunikation abgeschlossen ist 
10. Wie kann man Unbuffered Channels nutzen, was den Kontrollfluss von Goroutinen betrifft? 
	- **Koordinieren von paralleler Verarbeitung:** Unbuffered Channels können verwendet werden, um parallele Goroutinen zu koordinieren, indem sie sicherstellen, dass mehrere Goroutinen auf eine gemeinsam genutzte Datenquelle zugreifen.
	- **Implementieren von "Workers-Pool"**: Unbuffered Channels können verwendet werden, um einen "Workers-Pool" zu implementieren, in dem Goroutinen als "Worker" bezeichnet werden, die Aufgaben aus einer gemeinsamen Queue abarbeiten und die Ergebnisse zurück an den Aufrufer senden. 

11. Beschreiben Sie, welche Varianten des select Befehls es in Go gibt. Es ist nicht die exakte Syntax dieser Befehlsvarianten gefragt, sondern eine Beschreibung des Konzepts (der Semantik) der Varianten.
	- In Go gibt es _drei Varianten_ des select Befehls:
		1.  **Der normale select-Befehl:** Dieser Befehl ermöglicht es, auf mehrere Channels gleichzeitig zu warten und das erste verfügbare Ergebnis auszuführen, das empfangen wird. Der select-Befehl blockiert, bis ein Ergebnis verfügbar ist.
		2.  **Der non-blocking select-Befehl:** Dieser Befehl ermöglicht es, auf mehrere Channels gleichzeitig zu warten, aber ohne den Aufruf zu blockieren. Wenn kein Ergebnis verfügbar ist, wird der select-Befehl sofort beendet, ohne auf ein Ergebnis zu warten.
		3.  **Der default-select-Befehl:** Dieser Befehl ermöglicht es, eine default-Aktion auszuführen, wenn kein Ergebnis von den angegebenen Channels empfangen wird. Dies kann verwendet werden, um zu vermeiden, dass der select-Befehl endlos blockiert, wenn kein Ergebnis verfügbar ist.
	-  Der select-Befehl ermöglicht es also, auf mehrere Channels gleichzeitig zu warten und das erste verfügbare Ergebnis auszuführen. Er kann blockieren oder nicht blockieren und ermöglicht auch die Ausführung einer default-Aktion, wenn kein Ergebnis verfügbar ist. 
## Erlang
12. Nennen Sie den wesentlichen Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems, der dazu führt, dass Erlang Prozesse nicht 1:1 auf Threads abgebildet werden.
	- Der wesentliche Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems ist, dass **Erlang Prozesse auf virtuellen Prozessoren (VMs) ausgeführt werden und nicht direkt auf den Threads** des Betriebssystems aufgebaut sind.

13. Nennen Sie den Namen des Nebenläufigkeits-Konzepts von Erlang und beschreiben Sie es
	- **The Actor Model** is a concurrency model in which independent entities, called actors, interact with each other by sending and receiving messages. Each actor has its own mailbox (a queue of messages), and an associated behavior. An actor can only process one message at a time, but it can handle multiple messages concurrently by creating new actors to process them.
	- Actors are also **completely isolated** from each other, and can only communicate through message passing. This means that actors do not share memory or state, and can only affect each other through message passing. This isolation makes it much easier to write concurrent and parallel code, as there are no shared resources to synchronize or protect. 

14. Begründen Sie, warum sich Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen lassen, ohne dass dies im Erlang Programm besonders berücksichtigt werden muss.
	- Erlang Prozesse lassen sich über mehrere im Netzwerk verbundene Rechner verteilen, weil Erlang ein verteiltes Betriebssystem ist. Es nutzt das **Konzept der virtuellen Prozessoren (VMs), die auf jedem Rechner im Netzwerk ausgeführt werden können**. Jede VM kann Tausende von Prozessen ausführen und verwalten.
	- Erlang Prozesse können Nachrichten direkt an andere Erlang Prozesse auf anderen Rechnern senden, ohne dass es besondere Anstrengungen erfordert, um die Verteilung im Programm zu berücksichtigen. Dies erfolgt durch die Verwendung von Prozess-Identifikatoren (PIDs), die eindeutig für jeden Prozess im gesamten Netzwerk sind.
	- Die Verteilung der Prozesse erfolgt automatisch durch die Erlang-VM, die sicherstellt, dass Prozesse auf den am besten geeigneten Rechnern ausgeführt werden und dass Nachrichten zwischen Prozessen auf verschiedenen Rechnern sicher übertragen werden.
	- _Zusammengefasst_ ermöglicht die Verwendung von virtuellen Prozessoren und das Konzept der Nachrichtenübertragung es Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen zu lassen, ohne dass es im Erlang Programm besonders berücksichtigt werden muss. 

15. In Erlang möchte eine Funktion, die als Client agiert, einer anderen Funktion, die als Server agiert, einen Request (eine Anfrage, als Nachricht) schicken. Nennen Sie zwei Möglichkeiten, wie der Client den Server als "Nachrichtenziel" finden bzw. identifizieren kann, so dass die Nachricht dann beim richtigen Empfänger ankommt.
	1.  Der Client kann den Server über seine **Prozess-ID** (PID) identifizieren und die Nachricht an diese PID senden.
	2. Der Client kann den Server über einen **registrierten Namen** identifizieren und die Nachricht an diesen Namen senden.
1. In einem Erlang Programm agiere eine nebenläufige Funktion als Client und eine andere nebenläufige Funktion als Server. Der Client schickt mehrere AnfrageNachrichten an den Server und erhält später mehrere Antwort-Nachrichten. Wie kann der Erlang Programmierer sicherstellen, dass der Client Code die empfangenen Nachrichten der richtigen entsprechenden (vorher geschickten) Anfrage zuordnen kann?
	- Der Erlang Programmierer kann eine eindeutige Identifikation in jeder Anfrage-Nachricht einfügen und die gleiche Identifikation in der entsprechenden Antwort-Nachricht wieder verwenden. Der Client kann dann die empfangenen Antwort-Nachrichten anhand dieser eindeutigen Identifikation der entsprechenden Anfrage zuordnen. 
	- Eine weitere Möglichkeit wäre, dass der Server eine Referenz zur Anfrage des Clients in der Antwort-Nachricht mitschickt und der Client diese Referenz verwendet, um die Anfrage und die Antwort zuordnen zu können. 
2. Beschreiben Sie die Best Practice Empfehlung, wie in Erlang Code mit Fehlersituationen umgegangen werden soll.
	- Verwendung von *Exceptions*
	-  Erstellen von **benutzerdefinierten Exceptions** für spezifische Fehlertypen
	-  Fehlerbehandlung sollte den **normalen Ablauf des Programms nicht beeinträchtigen**
	-  Verwendung von Protokollen und Log-Dateien, um Fehler aufzuzeichnen und zu analysieren
	-  Prozesse erstellen die sich spezifisch um die Fehlerbehandlung kümmern
1. Bei der Erlang Programmierung gilt die Empfehlung: "Let it crash". Beschreiben Sie, wie im Erlang Code trotz crashender Prozesse die Fehlersituation eingegrenzt und strukturiert behandelt werden kann, so dass es nicht zu einem Crash des gesamten Applikationscodes kommen muss.
	- Ein wichtiger Bestandteil dieses Ansatzes ist die Verwendung von Supervisoren, die überwachen, welche Prozesse laufen und automatisch neue Instanzen von gecrashten Prozessen erstellen, um die Verfügbarkeit der Anwendung sicherzustellen. Supervisoren können auch konfiguriert werden, um bestimmte Aktionen auszuführen, wenn ein Prozess abstürzt, z.B. Benachrichtigungen versenden oder Protokolleinträge erstellen.
	- Außerdem können Erlang-Prozesse auch durch den Austausch von Nachrichten miteinander kommunizieren. Dies ermöglicht es, dass Prozesse unabhängig voneinander arbeiten und sich gegenseitig nicht beeinträchtigen, selbst wenn einer abstürzt. 
2. . Was versteht man im Erlang OTP Framework unter einem Supervisor Prozess und einem Worker Prozess, wenn es um die Behandlung von Fehlersituationen im Erlang Code geht? Welche Rolle spielen die beiden im Kontext der in Erlang programmierten Applikation?
	- Der Supervisor-Prozess spielt eine wichtige Rolle dabei die Verfügbarkeit der Anwendung aufrechtzuerhalten und die Ausführung der Worker-Prozesse sicherzustellen. 
	- Der Worker-Prozess ist für die Ausführung der Anwendungslogik zuständig und kann bei Fehlern abstürzen, aber der Supervisor-Prozess wird ihn automatisch neu erstellen und die Ausführung fortsetzen. 
3. Wofür benutzt das Erlang OTP Framework die Restart Strategien? Beschreiben Sie zwei verschiedene dieser Strategien, inklusive Beschreibung eines von Ihnen erdachten Anwendungsfalls, in dem die jeweilige Strategie angemessen ist.
	- Eine Restart-Strategie ist *"one_for_one"*. Diese Strategie ermöglicht es dem Supervisor-Prozess, jeden gecrashten Worker-Prozess einzeln neu zu starten. Diese Strategie eignet sich besonders für Anwendungen, bei denen jeder Worker-Prozess unabhängig von den anderen arbeitet und ein Absturz eines Prozesses keine Auswirkungen auf die anderen Prozesse hat. Ein Beispiel hierfür kann ein Anwendung sein, die eine grosse Anzahl von parallelen Verbindungen zu einer Datenbank aufrechterhält. Wenn eine Verbindung abstürzt, wird sie automatisch durch einen neuen Prozess ersetzt und die anderen Verbindungen bleiben aktiv.
	- Eine andere Restart-Strategie ist *"temporary"*. Diese Strategie ermöglicht es dem Supervisor-Prozess, gecrashte Worker-Prozesse temporär neu zu starten. Dies bedeutet, dass ein gecrashter Prozess nur für eine begrenzte Anzahl von Neustarts wiederbelebt wird, bevor der Supervisor-Prozess beschließt, dass der Prozess nicht mehr gestartet werden kann. Diese Strategie eignet sich besonders für Anwendungen, bei denen ein Prozess in einen unvermeidlichen Fehlerzustand geraten kann und weiterhin Neustarts nicht sinnvoll sind. 
		- Ein **Beispiel** für die Anwendung dieser Restart-Strategie kann die Steuerung von industriellen Prozessen in einer Fabrik sein, bei denen es wichtig ist dass die Verbindungen zu den Maschinen aufrechterhalten werden und bei Ausfällen schnell reagiert werden kann.
1. Bei Erlang Applikationen, die auf das Framework OTP aufgebaut sind, besteht die Möglichkeit, bei Teilen der Applikation zur Laufzeit Code auszutauschen. Beschreiben Sie, welche Eigenschaften Erlang als Sprache besitzt, auf die diese Möglichkeiten dann aufbauen können.
	- Dynamische Code-Änderung: Erlang ermöglicht es, Code zur Laufzeit zu ändern, indem es die Möglichkeit bietet, Code-Module nachzuladen und zu ersetzen, ohne dass der Prozess gestoppt werden muss.
	-   Hot-Swapping: Erlang ermöglicht es, Code-Module und Datenstrukturen während der Ausführung zu ändern, ohne dass der Prozess gestoppt werden muss. 
1. JavaScript Interpreter bieten im Prinzip nur eine "Ausführungseinheit" für den JavaScript Code und nur eine Event Loop. Trotzdem ist es möglich, nebenläufige Programmteile zu programmieren.
	1. Beschreiben Sie einen Mechanismus, durch den bestimmt wird, ob und wann die nebenläufigen Programmteile zur Ausführung kommen. 
	2. Zu welchen negativen Konsequenzen können die Einschränkungen "eine Ausführungseinheit" und "eine Event Loop" trotz des Vorhandenseins von Nebenläufigkeit führen, wenn man ausschließlich die Ausführung von JavaScript Code betrachtet? Denken Sie sich ein Beispiel aus, welches diesen Effekt illustriert, und beschreiben Sie es high-level.

## JavaScript

23. Was versteht man unter dem JavaScript Callback Programmierstil? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das Callback Prinzip illustriert, und erläutern Sie ihr Beispiel.
24. Wie kann man bei der JavaScript Programmierung mittels einfacher Callbacks die Fehlerbehandlung programmieren? Geben Sie ein JavaScript Codebeispiel mit einer von ihnen erdachten JavaScript Callback Funktion an, welches illustriert, wie die Fehlerinformationen einer asynchronen Operation an ein Callback übergeben und von diesem verarbeitet werden könnten. Das Beispiel soll auch die Registrierung der Callback Funktion beinhalten.
25. Was versteht man bei der JavaScript Programmierung unter dem Problem der "Callback Hell" (oder "Pyramid of Doom")? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das "Callback Hell" Problem illustriert, und erläutern Sie ihr Beispiel.
26. Erläutern Sie anhand eines von Ihnen erdachten JavaScript Pseudo-Code-Beispiels das Konzept des "Reactive Programming". Erläutern Sie insbesondere, auf welcher Art von Datenstrukturen dieser Ansatz basiert und welche Art von Operationen auf diesen Daten benutzt werden. Beschreiben Sie, welchen Vorteil dieser Programmieransatz bietet gegenüber der klassischen event-basierten Programmierung in JavaScript.
27. Was versteht man bei der JavaScript Programmierung unter dem Konzept der Promises? Was ist eine Promise? Wie wird sie im JavaScript Code programmiert? Geben Sie ein von Ihnen erdachtes JavaScript Pseudo-Code-Beispiel an, in dem eine von Ihnen erdachte JavaScript Funktion eine Promise erzeugt und zurückgibt. Erläutern Sie den Zweck der Promise mittels ihres Beispiels. Erläutern Sie auch, aus welchen Codekonstrukten sich die Promise zusammensetzt und wie diese Konstrukte in ihrem Beispiel genutzt werden, damit die Promise ihren Zweck erfüllen kann.
28. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise die Funktion resolve() einmal mit dem Parameterwert 11 aufgerufen wird (resolve(11)) und anschließend mit dem Parameterwert 22 aufgerufen wird (resolve(22))? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise zuerst resolve(33) aufgerufen wird und anschließend reject(-1) (d.h. Fehlercode -1)? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt.
29. Es ist im JavaScript Code auf keine der folgenden alternativen Arten möglich, die Beispiel-Promise zu resolven: 
	``` Javascript
	let my_promise = new Promise( … ); 
	resolve(my_promise); //funktioniert nicht 
	my_promise.resolve(42); // funktioniert auch nicht 
	```
	Erläutern Sie, wie bzw. von wo aus die Promise denn dann resolved werden kann und warum das Promise Konzept nicht erlaubt, die Promise auf die obigen zwei Arten zu resolven. Sie können in ihren Erläuterungen auch gerne den oben weggelassenen Teil des Codes (die drei Pünktchen) beispielhaft konkretisieren, wenn dies für ihre Erläuterungen notwendig sein sollte.
30. Wozu dient die .then() Methode bei JavaScript Promises? Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .then() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt.
31. Wozu dient die .catch() Methode bei JavaScript Promises? Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .catch() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt. Erläutern Sie ferner die Relation zwischen der .catch() Methode von Promises und dem catch in try/catch Blöcken. Wie ist die Relation zwischen diesen beiden Konstrukten im Kontext der Programmierung mit Promises? Sollten beide benutzt werden?
32. Was versteht man unter der Promise Absorption? Erläutern Sie, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt. Was versteht man unter Promise Chaining? Erläutern Sie auch hier, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt.
33. Gegeben eine JavaScript Promise, die schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf dieser Promise werde mittels der .then() Methode ein Callback registriert.  
``` JavaScript
let promise1 = Promise.resolve(42); promise1.then( result => { console.log("Resultat: " + result); } ); console.log("Skript Ende!");
```
	Wann wird der Callback ausgeführt werden? Geben Sie die relevanten Mechanismen
	des JavaScript Interpreters bzw. der JavaScript Plattform an, die zu diesem
	Verhalten führen.
34. Erläutern Sie das Konzept der Promise.all() Operation: Wozu wird diese Operation genutzt? Was sind die Parameter und was ist der Rückgabewert dieser Operation? Wie werden Fehlerszenarien durch diese Operation behandelt?
35. Was versteht man unter der "Promisification" einer JavaScript Library? Muss dazu die Library immer komplett neu geschrieben werden? Oder gibt es ein alternatives, effizienteres Vorgehen, dass sich bei manchen Libraries umsetzen ließ? Beschreiben Sie mittels JavaScript Pseudo-Code, wie dieses alternative, effiziente Verfahren funktioniert.
36. Beschreiben Sie anhand eines von Ihnen erdachten illustrativen JavaScript Codebeispiels, was das JavaScript async / await Konstrukt bei einer asynchronen, vom Programmierer definierten Funktion genutzt wird. Wofür wird der await Befehl genutzt, d.h. welche Funktion hat er in der technischen Realisierung der Nebenläufigkeit in JavaScript. Erläutern Sie dies insbesondere an ihrem Codebeispiel. Welche Rolle spielt das async Schlüsselwort für den Code der asynchronen Funktion bzw. für ihre Abarbeitung?
37. Beschreiben Sie für das JavaScript async / await Konstrukt, was die async Funktion für einen Rückgabewert besitzt. Erläutern Sie, welche Rolle der Rückgabewert der Funktion und der Rumpf der Funktion im Kontext der Nebenläufigkeit in JavaScript haben.
38. Erläutern Sie den Vorteil für den Programmierer der JavaScript Programmierung mittels async / await gegenüber einer Programmierung ausschließlich mit Promises (ohne Nutzung von async / await).
39. Gegeben eine JavaScript Promise, die innerhalb einer async Funktion definiert wird und schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf diese Promise werde mittels des await Befehls gewartet. async function fun_async() { let promise = Promise.resolve(42); let result = await promise; console.log(result); return result; } let promise2 = fun_async(); // ... Wann wird der Code hinter dem await ausgeführt werden? Geben Sie die relevanten Mechanismen des JavaScript Interpreters bzw. der JavaScript Plattform an, die zu diesem Verhalten führen.
40. Erläutern Sie den Vorteil der JavaScript Programmierung mittels des async / await Konstrukts gegenüber einer Programmierung ausschließlich mit Promises, wenn es mehrere asynchrone Operationen gibt, die über Daten voneinander abhängen, d.h. jede Operation benötigt das Ergebnis der Vorgängeroperation. Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.
41. Erläutern Sie das Konzept der Fehlerbehandlung beim JavaScript async / await Konstrukt. Wie ist die Relation zur Fehlerbehandlung bei der Programmierung (ausschließlich) mittels Promises und wie ist die Relation zur Fehlerbehandlung bei sequentiellen (nicht-nebenläufigen) JavaScript Code? Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.
42. Erläutern Sie das Konzept der objekt-basierten / prototypischen Vererbung in JavaScript. Welche Operation ist die zentrale Operation bei dieser Art der ObjektOrientierung, um das Konzept der Vererbung zu realisieren (es existieren mehrere Standardnamen für diese Operation, geben sie mindestens einen dieser Namen an)? Schreiben Sie JavaScript Code, um folgendes (ausschließlich) mittels dieser Art der Objekt-Orientierung zu realisieren: Alle Shape2D haben eine x und eine y Koordinate und unterstützen die parameterlose Methode mirror(). Der Rumpf der Methode kann leer gelassen werden. Rectangle erbt von Shape2D und bietet zusätzlich die parameterlose Methode area(). Auch hier kann der Methodenrumpf leergelassen werden. Square erbt von rectangle und überschreibt die Methode area(). my_square1 ist ein Square, bei dem sie dann x auf den Wert 10 und y auf den Wert 20 setzen (über direkte zuweisungen an die Attribute).
43. Was versteht man unter prototypischer Vererbung (auch objekt-basierte Vererbung genannt) in JavaScript? Nennen und beschreiben Sie drei charakteristische Aspekte, die wesentlich in diesem Konzept sind. Dies können z.B. Aspekte sein, wie dieses Konzept „funktioniert“ oder z.B. zentrale Konstrukte der Programmiersprache, die in diesem Konzept relevant sind.
44. Was versteht man unter pseudo-klassischer Vererbung in JavaScript? Nennen und beschreiben Sie auch hier drei charakteristische Aspekte, die wesentlich in diesem Konzept sind.
45. Gegeben sei folgender JavaScript Code. Zeichnen Sie das Diagramm der Objekte und ihrer Attribut-Beziehungen zueinander, dass sich bei der JavaScript-internen Umsetzung dieses Programmcodes ergibt. Orientieren Sie sich an der in der Vorlesung verwendeten Diagramm-Notation.
``` JavaScript
function Rectangle(length, width) { 
	this.length = length; 
	this.width = width; 
} 
Rectangle.prototype.getArea = function() { 
	return this.length * this.width; 
}; 
Rectangle.prototype.toString = function() { 
	return "[Rectangle " + this.length + "x" + this.width + "]"; 
}; 
var rect = new Rectangle(5, 10); 
function Square(size) { 
	Rectangle.call(this, size, size); 
} 
Square.prototype = new Rectangle(); 
Square.prototype.constructor = Square; 
Square.prototype.toString = function() { 
	return "[Square " + this.length + "x" + this.width + "]"; 
}; 
var square = new Square(6);
```
46. Erläutern Sie, was man bei JavaScript unter dem Begriff des "Polyfilling" versteht. (Den Code für das Polyfilling von Object.create() werde ich nicht erfragen. Auch nicht, wie man auf die Existenz testet und dann beim "fehlen" das Polyfilling macht. Aber als Beispiel für Polyfilling sollte man Object.create angeben können.)