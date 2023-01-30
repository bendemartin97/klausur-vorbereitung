**Donnerstag 2. Februar 2023, 8:30-10:30 Uhr.** 
**Dauer der Klausur: 120 Minuten**

**Wichtig!:**
Diese Präsentation umfasst nicht alle Themen des Semesters aus meinem Teil. Somit gibt es auch nicht zu allen Teilen einen „Beispiel-Fragenkatalog“. Die anderen Themen sind aber genauso prüfungsrelevant: Alle meine Themen aus dem Semester sind komplett prüfungsrelevant! Die Idee der Prüfungsinfos ist ja, Ihnen konkret ein Verständnis zu geben, wonach ich in schriftlichen bzw. mündlichen KMPS Prüfungen frage, d.h. wie sie sich für die Prüfungen vorbereiten müssen / sollten. Dies wird beispielhaft anhand ausgewählter Themen des Semesters konkret gemacht. Dieselbe „Idee der Art zu fragen“ gilt dann auch für alle meine anderen Themen des Semesters!

# Fragen/Aufgaben

## Go / Concurrency / Parallelismus

1. Beschreiben Sie, was man unter **Concurrency** und was man unter **Parallelismus** versteht. Grenzen Sie die beiden gegeneinander ab
	-  *Concurrency* beschreibt die Fähigkeit eines Systems, mehrere Aufgaben **gleichzeitig zu verwalten**. Es geht darum, dass verschiedene Aufgaben **parallel ausgeführt werden, aber nicht unbedingt gleichzeitig auf einem einzigen Prozessor.** Ein Beispiel für Concurrency ist das Verwalten von mehreren Anwendungen auf einem Computer, wobei jede Anwendung im Hintergrund läuft und die CPU-Zeit teilt.
	- *Parallelismus* beschreibt die Fähigkeit eines Systems, mehrere Aufgaben **gleichzeitig auf mehreren Prozessoren oder Kerne auszuführen**. Es geht darum, dass die Aufgaben **tatsächlich gleichzeitig auf unterschiedlichen Prozessoren** ausgeführt werden, um die Leistung zu steigern. Ein Beispiel für Parallelismus ist die Verwendung von mehreren Kerne in einem Computer, um die Ausführung einer Aufgabe zu beschleunigen.
	- Concurrency und Parallelismus sind eng miteinander verbunden, aber nicht dasselbe. Concurrency konzentriert sich darauf, wie Aufgaben verwaltet werden, während Parallelismus sich darauf konzentriert, wie Aufgaben ausgeführt werden. **Ein System kann concurrency haben, ohne parallelismus zu haben, aber es kann nicht parallelismus haben, ohne concurrency zu haben.**

2. Beschreiben Sie das **Scheduling nebenläufiger Programmteile** in **Go** und in **Erlang** und grenzen Sie diese beiden Scheduling Konzepte gegeneinander ab. 
	- Das Scheduling in Go und Erlang unterscheidet sich in der Art und Weise, wie sie mit Threads umgehen. **Go verwendet ein M:N-Modell**, bei dem eine **feste Anzahl von Os-Threads** verwendet wird, um eine **dynamische Anzahl von Goroutinen** auszuführen. **Erlang verwendet ein 1:1-Modell,** bei dem **jeder Prozess einen Os-Thread verwendet**. _Dies bedeutet, dass Erlang in der Lage sein kann, mehr Prozesse auf einer begrenzten Anzahl von Os-Threads auszuführen, während Go möglicherweise mehr Os-Threads verwendet, um die gleiche Anzahl von Goroutinen auszuführen._
	- Beide Scheduler haben ihre eigenen Vorteile und Nachteile. **Erlang's 1:1-Modell** ermöglicht es, mehr Prozesse auf einer begrenzten Anzahl von Os-Threads auszuführen und ermöglicht es Prozessen, schnell und effizient zu kommunizieren, während Go's M:N-Modell es ermöglicht, mehrere Kerne besser auszunutzen und es ermöglicht es Goroutinen, schnell und effizient zu kontextwechseln. 

3. Was versteht man unter **kooperativem Scheduling**? Nennen Sie dabei auch einen Vorteil und einen Nachteil dieses Scheduling Ansatzes. Beschreiben Sie das kooperative Scheduling in der Programmiersprache **Go**.
	- Kooperatives Scheduling bezieht sich auf eine Art von Scheduling, bei der die verwalteten Aufgaben, wie z.B. **Threads, aktiv daran mitwirken, den Zeitpunkt des Kontextwechsels zu bestimmen.** Im Gegensatz dazu wird bei **präemptiven** Scheduling der **Scheduler entscheiden, wann der Kontext einer Aufgabe gewechselt wird.**
	- Ein _Vorteil_ von kooperativem Scheduling ist, dass es die Programmierung einfacher macht, da die Entwickler die Kontrolle darüber haben, wann ein Kontextwechsel stattfindet. Dies ermöglicht es ihnen, ihre Anwendungen *besser zu optimieren* und *potenzielle Probleme mit Deadlocks oder anderen Formen von Blockierungen zu vermeiden.*
	- Ein _Nachteil_ von kooperativem Scheduling ist, dass es *leicht zu Fehlern* führen kann, wenn Entwickler die Kontrolle über den Kontextwechsel nicht richtig ausüben. Eine Aufgabe kann zum Beispiel in eine *Endlosschleife* geraten, ohne jemals den Kontext freizugeben, was zu Blockierungen und Leistungsproblemen führen kann.
	- Goroutinen sind ein Beispiel für kooperatives Scheduling. Goroutinen ermöglichen es Entwicklern, nebenläufige Aufgaben zu erstellen, indem sie das Schlüsselwort "go" verwenden. Goroutinen teilen sich einen **gemeinsamen Stack** und die Ausführung **findet kooperativ statt**, das heißt, dass eine **Goroutine dem Scheduler signalisiert, dass sie bereit ist, den Kontext freizugeben,** bevor sie blockiert oder einen anderen Zustand erreicht, in dem sie nicht weiter ausgeführt werden kann.

4. Wie wird der **Wechsel der Kontrolle** zwischen den **nebenläufigen Goroutinen** eines Go Programms veranlasst? Muss der Programmierer des Programms dies immer selbst programmieren?
	- Der Programmierer muss dies **nicht immer selbst programmieren**, da Go eine hochgradig parallele Programmierung unterstützt und die Verwaltung von Goroutinen automatisch übernimmt. Der Programmierer **kann jedoch gezielt den Wechsel der Kontrolle steuern, indem er "channel"-Datentypen verwendet**, um die Kommunikation zwischen Goroutinen zu steuern und Synchronisation zu erreichen.

5. Beschreiben Sie die Konzepte der Programmiersprachen **Go und Erlang betreffend geteilte Daten zwischen nebenläufigen Programmteilen.**
	-   Go verwendet **Goroutinen** für nebenläufige Programmteile und teilt **denselben Speicherbereich.** Synchronisation von Zugriffen auf gemeinsam genutzte Daten erfolgt mittels Mutexes, Semaphoren und Channels.
	- Erlang verwendet **Prozesse** für nebenläufige Programmteile und jeder Prozess hat **seinen eigenen Speicherbereich**. Synchronisation erfolgt durch **message passing** und das Konzept der "nicht-gefährlichen paralleler Programmierung"
	- _Go hat explizite Synchronisationsmechanismen, Erlang hat keine explizite Synchronisation._ <small>(Zugriff auf die Daten wird automatisch synchronisieren, ohne dass es zu Fehlern oder inkonsistenten Zuständen kommt.)</small>
	- _In Go gibt es die Möglichkeit zu Deadlocks und Kollisionen, Erlang hat keine Deadlocks oder Kollisionen._

6. Nennen Sie den wesentlichen **Unterschied zwischen Goroutinen und den Threads des zugrundeliegenden Betriebssystems**, der dazu führt, dass Goroutinen nicht 1:1 auf Threads abgebildet werden.
	-   Goroutinen verwaltet auf Benutzerseite, Threads von Betriebssystem-Kernel
	-   Goroutinen leichter und schneller zu erstellen und verwalten
	-   Goroutinen benötigen weniger Systemressourcen und verursachen weniger Overhead
	-   Goroutinen teilen sich denselben Adressraum, einfacher auf gemeinsame Datenstrukturen zugreifen
	-   Threads haben ihren eigenen Adressraum, müssen synchronisiert werden um auf gemeinsame Daten zugreifen zu können. 

7. Was versteht man in **Go** unter einem **Tight Loop Szenario**? Geben Sie ein Beispiel (Go Codeausschnitt) dafür an. Wie kann der Programmierer dieses Problem lösen?
	- Ein Tight Loop Szenario in Go bezieht sich auf den Fall, in dem eine **Goroutine in einer Schleife (loop) hängt**, die sehr oft ausgeführt wird, ohne dass sie den Kontext freigibt. Dies kann dazu führen, dass die Goroutine die CPU-Zeit dominiert und andere Goroutinen nicht ausgeführt werden können, was zu Leistungsproblemen führen kann.
	- Der Programmierer kann das Problem eines Tight Loop Szenarios lösen, indem er die Goroutine anweist, den Kontext freizugeben, mit z.B. `runtime.Gosched()`. Oder man kann die Berechnungen in kleinere Blöcke aufteilen und diese mit einem channel oder WaitGroup synchronisieren.

8. Beschreiben Sie, wofür **Channels in Go** genutzt werden. Welche Arten von Channels gibt es? Für welche Kommunikationsszenarien werden diese genutzt? Mittels welcher Operationen können die Goroutinen solche Channels nutzen? 
	- Es gibt zwei Arten von Channels in Go: **unidirektionale und bidirektionale Channels**. Unidirektionale Channels ermöglichen es Goroutinen, nur Daten zu senden oder nur Daten zu empfangen. Bidirektionale Channels ermöglichen es Goroutinen, sowohl Daten zu senden als auch zu empfangen.
	- Channels werden in Go genutzt, um die **Kommunikation und Synchronisation zwischen Goroutinen** in verschiedenen Szenarien zu ermöglichen. Beispiele sind:
		- das Senden von Daten von einer Goroutine an eine andere Goroutine,
		- das Synchronisieren von Goroutinen, indem eine Goroutine auf eine Nachricht wartet, bevor sie fortfährt,
		- das Koordinieren von paralleler Verarbeitung, indem mehrere Goroutinen auf eine gemeinsam genutzte Datenquelle zugreifen.
	- Goroutinen können Channels nutzen, indem sie die **Operationen "make", "send" und "receive" verwenden.**
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
	- Dies ist das Verhalten, das als _"synchronized send and receive"_ bezeichnet wird, **da sowohl Sender als auch Empfänger blockieren**, bis die Kommunikation abgeschlossen ist 
10. Wie kann man Unbuffered Channels nutzen, was den Kontrollfluss von Goroutinen betrifft? 
	- **Koordinieren von paralleler Verarbeitung:** Unbuffered Channels können verwendet werden, um parallele Goroutinen zu koordinieren, indem sie sicherstellen, dass mehrere Goroutinen auf eine gemeinsam genutzte Datenquelle zugreifen.

11. Beschreiben Sie, welche **Varianten des select Befehls es in Go** gibt. Es ist nicht die exakte Syntax dieser Befehlsvarianten gefragt, sondern eine Beschreibung des Konzepts (der Semantik) der Varianten.
	- In Go gibt es _drei Varianten_ des select Befehls:
		1.  **Der normale select-Befehl:** Dieser Befehl ermöglicht es, auf mehrere Channels gleichzeitig zu warten und das erste verfügbare Ergebnis auszuführen, das empfangen wird. Der select-Befehl blockiert, bis ein Ergebnis verfügbar ist.
		2.  **Der non-blocking select-Befehl:** Dieser Befehl ermöglicht es, auf mehrere Channels gleichzeitig zu warten, aber ohne den Aufruf zu blockieren. Wenn kein Ergebnis verfügbar ist, wird der select-Befehl sofort beendet, ohne auf ein Ergebnis zu warten.
		3.  **Der default-select-Befehl:** Dieser Befehl ermöglicht es, eine default-Aktion auszuführen, wenn kein Ergebnis von den angegebenen Channels empfangen wird. Dies kann verwendet werden, um zu vermeiden, dass der select-Befehl endlos blockiert, wenn kein Ergebnis verfügbar ist.
	-  Der select-Befehl ermöglicht es also, auf mehrere Channels gleichzeitig zu warten und das erste verfügbare Ergebnis auszuführen. Er kann blockieren oder nicht blockieren und ermöglicht auch die Ausführung einer default-Aktion, wenn kein Ergebnis verfügbar ist.

## Erlang
12. Nennen Sie den wesentlichen **Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems**, der dazu führt, dass Erlang Prozesse nicht 1:1 auf Threads abgebildet werden.
	- Der wesentliche Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems ist, dass **Erlang Prozesse auf virtuellen Prozessoren (VMs) ausgeführt werden und nicht direkt auf den Threads** des Betriebssystems aufgebaut sind.

13. Nennen Sie den Namen des Nebenläufigkeits-Konzepts von Erlang und beschreiben Sie es
	- **The Actor Model** is a concurrency model in which **independent entities, called actors, interact with each other by sending and receiving messages**. Each actor has its **own mailbox (a queue of messages),** and an associated behavior. An actor can only process **one message at a time**, but it can **handle multiple messages concurrently by creating new actors to process them.**
	- Actors are also **completely isolated** from each other, and can only communicate through **message passing**. This means that actors do not share memory or state, and can only affect each other through message passing. This isolation makes it much easier to write concurrent and parallel code, as there are no shared resources to synchronize or protect. 

14. Begründen Sie, warum sich Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen lassen, ohne dass dies im Erlang Programm besonders berücksichtigt werden muss.
	- Erlang Prozesse lassen sich über mehrere im Netzwerk verbundene Rechner verteilen, weil Erlang ein verteiltes Betriebssystem ist. Es nutzt das **Konzept der virtuellen Prozessoren (VMs), die auf jedem Rechner im Netzwerk ausgeführt werden können**. Jede VM kann Tausende von Prozessen ausführen und verwalten.
	- Erlang Prozesse können Nachrichten direkt an andere Erlang Prozesse auf anderen Rechnern senden, ohne dass es besondere Anstrengungen erfordert, um die Verteilung im Programm zu berücksichtigen. **Dies erfolgt durch die Verwendung von Prozess-Identifikatoren (PIDs), die eindeutig für jeden Prozess im gesamten Netzwerk sind.**
	- Die Verteilung der Prozesse erfolgt automatisch durch die Erlang-VM, die sicherstellt, dass Prozesse auf den am besten geeigneten Rechnern ausgeführt werden und dass Nachrichten zwischen Prozessen auf verschiedenen Rechnern sicher übertragen werden.
	- _Zusammengefasst_ ermöglicht die Verwendung von virtuellen Prozessoren und das Konzept der Nachrichtenübertragung es Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen zu lassen, ohne dass es im Erlang Programm besonders berücksichtigt werden muss. 

15. In Erlang möchte eine Funktion, die als Client agiert, einer anderen Funktion, die als Server agiert, einen Request (eine Anfrage, als Nachricht) schicken. **Nennen Sie zwei Möglichkeiten, wie der Client den Server als "Nachrichtenziel" finden bzw. identifizieren kann,** so dass die Nachricht dann beim richtigen Empfänger ankommt.
	1.  Der Client kann den Server über seine **Prozess-ID** (PID) identifizieren und die Nachricht an diese PID senden.
	2. Der Client kann den Server über einen **registrierten Namen** identifizieren und die Nachricht an diesen Namen senden.
1. In einem Erlang Programm agiere eine nebenläufige Funktion als Client und eine andere nebenläufige Funktion als Server. Der Client schickt mehrere AnfrageNachrichten an den Server und erhält später mehrere Antwort-Nachrichten. Wie kann der Erlang Programmierer sicherstellen, dass der Client Code die empfangenen Nachrichten der richtigen entsprechenden (vorher geschickten) Anfrage zuordnen kann?
	- Der Erlang Programmierer kann eine **eindeutige Identifikation in jeder Anfrage-Nachricht** einfügen und die gleiche Identifikation in der entsprechenden Antwort-Nachricht wieder verwenden. Der Client kann dann die empfangenen Antwort-Nachrichten anhand dieser eindeutigen Identifikation der entsprechenden Anfrage zuordnen. 
	- Eine weitere Möglichkeit wäre, dass der Server eine Referenz zur Anfrage des Clients in der Antwort-Nachricht mitschickt und der Client diese Referenz verwendet, um die Anfrage und die Antwort zuordnen zu können. 
2. Beschreiben Sie die **Best Practice Empfehlung**, wie in Erlang Code mit **Fehlersituationen** umgegangen werden soll.
	- Verwendung von *Exceptions*
	-  Erstellen von **benutzerdefinierten Exceptions** für spezifische Fehlertypen
	-  Fehlerbehandlung sollte den **normalen Ablauf des Programms nicht beeinträchtigen**
	-  Verwendung von Protokollen und Log-Dateien, um Fehler aufzuzeichnen und zu analysieren
	-  Prozesse erstellen die sich spezifisch um die Fehlerbehandlung kümmern
1. Bei der Erlang Programmierung gilt die Empfehlung: "**Let it crash**". Beschreiben Sie, wie im Erlang Code trotz crashender Prozesse die Fehlersituation eingegrenzt und strukturiert behandelt werden kann, so dass es nicht zu einem Crash des gesamten Applikationscodes kommen muss.
	- Ein wichtiger Bestandteil dieses Ansatzes ist die Verwendung von **Supervisoren**, die überwachen, **welche Prozesse laufen und automatisch neue Instanzen von gecrashten Prozessen erstellen**, um die Verfügbarkeit der Anwendung sicherzustellen. Supervisoren können auch **konfiguriert werden, um bestimmte Aktionen auszuführen**, wenn ein Prozess abstürzt, z.B. Benachrichtigungen versenden oder Protokolleinträge erstellen.
	- Außerdem können Erlang-Prozesse auch durch den Austausch von Nachrichten miteinander kommunizieren. Dies ermöglicht es, dass Prozesse unabhängig voneinander arbeiten und sich gegenseitig nicht beeinträchtigen, selbst wenn einer abstürzt. 
2. . Was versteht man im Erlang OTP Framework unter einem **Supervisor Prozess und einem Worker Prozess, wenn es um die Behandlung von Fehlersituationen** im Erlang Code geht? Welche Rolle spielen die beiden im Kontext der in Erlang programmierten Applikation?
	- Der _Supervisor-Prozess_ spielt eine wichtige Rolle dabei die **Verfügbarkeit der Anwendung aufrechtzuerhalten** und die Ausführung der Worker-Prozesse sicherzustellen. 
	- Der _Worker-Prozess_ ist für die **Ausführung der Anwendungslogik zuständig** und kann bei Fehlern abstürzen, aber der Supervisor-Prozess wird ihn automatisch neu erstellen und die Ausführung fortsetzen. 
3. Wofür benutzt das Erlang OTP Framework die **Restart Strategien?** Beschreiben Sie zwei verschiedene dieser Strategien, inklusive Beschreibung eines von Ihnen erdachten Anwendungsfalls, in dem die jeweilige Strategie angemessen ist.
	- Eine Restart-Strategie ist *"one_for_one"*. Diese Strategie ermöglicht es dem **Supervisor-Prozess, jeden gecrashten Worker-Prozess einzeln neu zu starten.** Diese Strategie eignet sich besonders für Anwendungen, bei denen jeder Worker-Prozess unabhängig von den anderen arbeitet und ein Absturz eines Prozesses keine Auswirkungen auf die anderen Prozesse hat. Ein Beispiel hierfür kann ein Anwendung sein, die eine grosse Anzahl von parallelen Verbindungen zu einer Datenbank aufrechterhält. Wenn eine Verbindung abstürzt, wird sie automatisch durch einen neuen Prozess ersetzt und die anderen Verbindungen bleiben aktiv.
	- Eine andere Restart-Strategie ist *"temporary"*. Diese Strategie ermöglicht es dem Supervisor-Prozess, **gecrashte Worker-Prozesse temporär neu zu starten.** Dies bedeutet, dass ein gecrashter Prozess nur für eine begrenzte Anzahl von Neustarts wiederbelebt wird, bevor der Supervisor-Prozess beschließt, dass der Prozess nicht mehr gestartet werden kann. Diese Strategie eignet sich besonders für Anwendungen, bei denen ein Prozess in einen unvermeidlichen Fehlerzustand geraten kann und weiterhin Neustarts nicht sinnvoll sind. 
		- Ein **Beispiel** für die Anwendung dieser Restart-Strategie kann die Steuerung von industriellen Prozessen in einer Fabrik sein, bei denen es wichtig ist dass die Verbindungen zu den Maschinen aufrechterhalten werden und bei Ausfällen schnell reagiert werden kann.
1. Bei Erlang Applikationen, die auf das Framework OTP aufgebaut sind, besteht die Möglichkeit, bei **Teilen der Applikation zur Laufzeit Code auszutauschen**. Beschreiben Sie, welche Eigenschaften Erlang als Sprache besitzt, auf die diese Möglichkeiten dann aufbauen können.
	- **Prozessorientierung**: Erlang verwendet ein Prozessmodell, bei dem jede Anwendung in viele kleine, unabhängige Prozesse unterteilt wird, die parallel ausgeführt werden können. Dies ermöglicht es, Teile der Anwendung zur Laufzeit zu isolieren und auszutauschen, ohne dass die anderen Teile beeinträchtigt werden.
	- **Fehlertoleranz**: Erlang ist fehlertolerant, da jeder Prozess seine eigene Ausführungsumgebung hat und Fehler innerhalb eines Prozesses die anderen Prozesse nicht beeinträchtigen. Dies ermöglicht es, Prozesse zur Laufzeit zu starten, zu stoppen und zu ersetzen, ohne dass die Anwendung insgesamt beeinträchtigt wird.
	- **Nachrichtenaustausch**: Erlang verwendet ein Nachrichtenaustauschmodell, bei dem Prozesse miteinander kommunizieren, indem sie Nachrichten über Kanäle senden und empfangen. Dies ermöglicht es, Prozesse zur Laufzeit zu ersetzen, ohne dass die Kommunikation zwischen Prozessen beeinträchtigt wird.
	- **Hot-Code-Loading**: Erlang unterstützt Hot-Code-Loading, d.h. es ist möglich, Code zur Laufzeit zu laden und auszuführen, ohne dass die Anwendung gestoppt werden muss. Dies ermöglicht es, Teile der Anwendung zur Laufzeit aktualisieren, ohne dass die Benutzer beeinträchtigt werden.

## JavaScript

22. JavaScript Interpreter bieten im Prinzip nur eine "Ausführungseinheit" für den JavaScript Code und nur eine Event Loop. Trotzdem ist es möglich, nebenläufige Programmteile zu programmieren.
	1. Beschreiben Sie einen Mechanismus, durch den bestimmt wird, ob und wann die nebenläufigen Programmteile zur Ausführung kommen. 
		- Ein Mechanismus, der bestimmt, ob und wann nebenläufige Programmteile zur Ausführung kommen, ist das **Konzept der asynchronen Programmierung**. Dabei werden bestimmte Aufgaben, wie das Lesen von Daten aus einer Datei oder das Senden einer Anfrage an einen Server, als asynchrone Funktionen ausgeführt. **Diese Funktionen geben sofort eine Antwort zurück, ohne auf die vollständige Ausführung der Aufgabe zu warten.** Stattdessen werden **Callbacks oder Promises** verwendet, um anzuzeigen, wann die Aufgabe abgeschlossen ist und die Ergebnisse verfügbar sind. 
	2. Zu welchen negativen Konsequenzen können die Einschränkungen "eine Ausführungseinheit" und "eine Event Loop" trotz des Vorhandenseins von Nebenläufigkeit führen, wenn man ausschließlich die Ausführung von JavaScript Code betrachtet? Denken Sie sich ein Beispiel aus, welches diesen Effekt illustriert, und beschreiben Sie es high-level.
		- Eine Einschränkung von JavaScript Interpretern ist, dass sie nur **eine Ausführungseinheit und eine Event Loop haben**. Das bedeutet, dass sie nur eine **Aufgabe zur gleichen Zeit ausführen** können. Wenn eine Aufgabe, die länger dauert, z.B. eine lange Schleife, ausgeführt wird, kann dies die Reaktionszeit anderer Aufgaben beeinträchtigen, die auf die gleiche Ausführungseinheit warten. Dies kann insbesondere bei Aufgaben, die auf Benutzereingaben reagieren, wie z.B. UI-Interaktionen, zu einer **schlechten Benutzererfahrung** führen. Ein Beispiel hierfür könnte eine Webanwendung sein, die große Datenmengen verarbeitet und dabei die Möglichkeit zur Interaktion mit der Anwendung blockiert, was zu Verzögerungen und Unzufriedenheit bei den Benutzern führen kann. 

23. Was versteht man unter dem JavaScript Callback Programmierstil? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das Callback Prinzip illustriert, und erläutern Sie ihr Beispiel.
	- Callback-Programmierung ist **ein Konzept der asynchronen Programmierung, bei dem eine Funktion (der "Callback") als Argument an eine andere Funktion übergeben wird, die später aufgerufen wird**. Dies ermöglicht es, dass eine Funktion ihre Arbeit fortsetzen kann, während eine andere Funktion ausgeführt wird.
	- *Example*
	
		```JavaScript
		function getDataFromServer(callback) {
		    // Simuliert einen Server-Aufruf, der Daten zurückgibt
		    setTimeout(() => {
		        const data = {
		            name: "John",
		            age: 30
		        };
		        callback(data);
		    }, 2000);
		}
		
		function displayData(data) {
		    console.log(data);
		}
		
		getDataFromServer(displayData); 
		// Gibt nach 2 Sekunden { name: "John", age: 30 } aus
		```
	- In diesem Beispiel ruft die Funktion `getDataFromServer` einen asynchronen Prozess auf, der simuliert, Daten von einem Server zu erhalten. Wenn die Daten verfügbar sind, ruft die Funktion den übergebenen Callback (`displayData`) auf und übergibt ihm die Daten. Die `displayData` Funktion gibt die Daten dann auf der Konsole aus.

1. Wie kann man bei der JavaScript Programmierung **mittels einfacher Callbacks die Fehlerbehandlung programmieren?** Geben Sie ein JavaScript Codebeispiel mit einer von ihnen erdachten JavaScript Callback Funktion an, welches illustriert, wie die Fehlerinformationen einer asynchronen Operation an ein Callback übergeben und von diesem verarbeitet werden könnten. Das Beispiel soll auch die Registrierung der Callback Funktion beinhalten.
	- In JavaScript kann man Fehler in Callbacks durch die Übergabe eines Error-Objekts als erstes Argument des Callbacks handhaben.
	- *Example*

		```Javascript
		function getDataFromServer(callback) {
		    // Simuliert einen Server-Aufruf, der Daten zurückgibt
		    setTimeout(() => {
			    // Simuliert einen erfolgreichen oder fehlerhaften Aufruf
		        const success = 
			        Math.random() >= 0.5;
			        
		        if (success) {
		            const data = { name: "John", age: 30 };
		            callback(null, data); // Kein Fehler, übergebe Daten
		            
		        } else {
			        const error = 
				        new Error("Could not retrieve data from server");
		            callback(error); // Fehler, übergebe Error-Objekt
		        }
		    }, 2000);
		}

		// Registrieren des Callbacks
		getDataFromServer(function (error, data) {
		    if (error) {
		        console.error(error.message);
		    } else {
		        console.log(data);
		    }
		});

		```
	- In diesem Beispiel wird das Ergebnis des Serveraufrufs simuliert. Wenn der Aufruf erfolgreich ist, werden die Daten an den Callback übergeben, ansonsten wird ein Error-Objekt übergeben. Der Callback prüft dann, ob ein Fehler vorliegt und handelt entsprechend. Wenn es ein Fehler ist, wird die Fehlermeldung auf der Konsole ausgegeben, andernfalls werden die Daten ausgegeben.

1. Was versteht man bei der JavaScript Programmierung unter dem Problem der **"Callback Hell" (oder "Pyramid of Doom")?** Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das "Callback Hell" Problem illustriert, und erläutern Sie ihr Beispiel.
	- Beim "Callback Hell" oder "Pyramid of Doom" handelt es sich um ein Problem, das durch eine **übermäßige Verwendung von Callbacks in JavaScript** entstehen kann. Dies führt zu einer **verschachtelten Struktur** von Callbacks, die schwer zu lesen, zu verstehen und zu warten ist.
	- *Example*

		```JavaScript
		function getUser(id, callback) {
		    // Simuliert einen Aufruf, um einen Benutzer zu erhalten
		    setTimeout(() => {
		        const user = { id: id, name: "John" };
		        callback(user);
		    }, 1000);
		}
		
		function getOrders(user, callback) {
		    // Simuliert einen Aufruf, 
		    //um Bestellungen für einen Benutzer zu erhalten
		    setTimeout(() => {
		        const orders = [
		            { id: 1, userId: user.id, product: "Widget" },
		            { id: 2, userId: user.id, product: "Gadget" },
		        ];
		        callback(orders);
		    }, 1000);
		}
		
		function getOrderDetails(order, callback) {
		    // Simuliert einen Aufruf, 
		    //um Details für eine Bestellung zu erhalten
		    setTimeout(() => {
		        const details = {
		            orderId: order.id,
		            product: order.product,
		            price: 10,
		        };
		        callback(details);
		    }, 1000);
		}
		
		// Callback Hell
		getUser(1, function (user) {
		    getOrders(user, function (orders) {
		        getOrderDetails(orders[0], function (details) {
		            console.log(details);
		        });
		    });
		});

		```

	- In diesem Beispiel werden die Funktionen `getUser`, `getOrders`, `getOrderDetails` verwendet, um Daten von einem Server zu erhalten. Jede Funktion nimmt einen Callback als Argument und ruft diesen auf, wenn die Daten verfügbar sind. Das Problem ist, dass die Callbacks innerhalb von Callbacks verkettet werden, was zu einer sehr tief verschachtelten Struktur führt, die schwer zu lesen und zu verstehen ist.
	- Um dieses Problem zu umgehen, gibt es verschiedene Lösungen. Eine Möglichkeit ist die Verwendung von Promises, um die asynchronen Aufrufe zu vereinfachen und die Struktur des Codes zu verbessern. Eine andere Möglichkeit ist die Verwendung von async/await, die einen synchronen Programmierstil ermöglichen und die Lesbarkeit und Verständlichkeit des Codes verbessern.
1. Erläutern Sie anhand eines von Ihnen erdachten JavaScript Pseudo-Code-Beispiels das Konzept des "**Reactive Programming**". Erläutern Sie insbesondere, auf welcher Art von Datenstrukturen dieser Ansatz basiert und welche Art von Operationen auf diesen Daten benutzt werden. Beschreiben Sie, welchen Vorteil dieser Programmieransatz bietet gegenüber der klassischen event-basierten Programmierung in JavaScript.
	- Reactive Programming ist ein Ansatz der Programmierung, bei dem Datenstrukturen verwendet werden, die sich **automatisch anpassen, wenn sich die Daten ändern.** Dies ermöglicht es, dass die Anwendung auf Veränderungen in den Daten reagieren und entsprechende Aktionen ausführen kann.
	- *Example*

		```JavaScript
		// Erstellen eines Observable-Objekts
		const data = Rx.Observable.of(1,2,3,4,5);
		
		// Anmelden einer Subscription
		data.subscribe(val => console.log(val));
		
		// Ausgabe: 1, 2, 3, 4, 5

		```

	- In diesem Beispiel wird ein Observable-Objekt erstellt, das eine Sequenz von Zahlen enthält. Mit der subscribe Methode wird eine Subscription erstellt, die jeden Wert des Observable-Objekts auf der Konsole ausgibt, sobald er emittiert wird.
	- Auf dieser Art von Datenstrukturen, die als Observable bezeichnet werden, kann man Operationen wie map, filter, reduce etc. anwenden, um die Daten zu transformieren, filtern oder zusammenzufassen.
	- Der Vorteil des Reactive Programming gegenüber der klassischen event-basierten Programmierung in JavaScript besteht darin, dass es eine bessere Abstraktion der Datenströme bietet und es einfacher ist, mit asynchronen Daten umzugehen. Es ermöglicht es auch, die Datenströme zu kombinieren und zu verketten, was zu einer einfacheren und saubereren Architektur führt.
1. Was versteht man bei der JavaScript Programmierung unter dem Konzept der **Promises**? Was ist eine Promise? Wie wird sie im JavaScript Code programmiert? Geben Sie ein von Ihnen erdachtes JavaScript Pseudo-Code-Beispiel an, in dem eine von Ihnen erdachte JavaScript Funktion eine Promise erzeugt und zurückgibt. Erläutern Sie den Zweck der Promise mittels ihres Beispiels. Erläutern Sie auch, aus welchen Codekonstrukten sich die Promise zusammensetzt und wie diese Konstrukte in ihrem Beispiel genutzt werden, damit die Promise ihren Zweck erfüllen kann.
	- Promises sind eine Möglichkeit, um asynchrone JavaScript-Operationen zu handhaben. **Eine Promise ist ein Objekt, das eine asynchrone Operation repräsentiert und das Ergebnis dieser Operation entweder erfolgreich (resolved) oder nicht erfolgreich (rejected) ist.** Promises ermöglichen es, die Verarbeitung von asynchronen Ergebnissen in einer sequentiellen und übersichtlichen Art und Weise zu handhaben.
	- *Example*

		```JavaScript
		function getDataFromServer() {
		  return new Promise((resolve, reject) => {
		    // Simuliert einen Server-Aufruf
		    setTimeout(() => {
			  // Simuliert einen erfolgreichen oder fehlerhaften Aufruf
		      const success = Math.random() >= 0.5; 
		      if (success) {
		        const data = { name: "John", age: 30 };
		        resolve(data); 
		      } else {
		        const error = 
			        new Error("Could not retrieve data from server");
		        reject(error);
		      }
		    }, 2000);
		  });
		}
		
		getDataFromServer()
		  .then(data => console.log(data))
		  .catch(error => console.error(error.message));

		```

	- In diesem Beispiel erstellen wir eine Funktion `getDataFromServer` die eine Promise zurückgibt. In der Funktion wird ein asynchroner Prozess simuliert und das Ergebnis entweder erfolgreich oder nicht erfolgreich ist. Wenn die Daten erfolgreich geholt werden, wird die resolve Methode aufgerufen und übergeben Daten, andernfalls wird die reject Methode aufgerufen und übergeben einen Fehler.
	- *Die Promise besteht aus 3 Teilen:*
		-   ein **Executor** (der Teil der Funktion, der die asynchrone Operation ausführt)
		-   eine **resolve Methode** (die aufgerufen wird, wenn die asynchrone Operation erfolgreich ist)
		-   eine **reject Methode** (die aufgerufen wird, wenn die asynchrone Operation nicht erfolgreich ist)
	- Die Methoden **then und catch** werden verwendet, um die **Verarbeitung des Ergebnisses der asynchronen Operation zu steuern**. Die then Methode wird aufgerufen, wenn die Operation erfolgreich ist und die catch Methode wird aufgerufen, wenn die Operation fehlschlägt.
	- Das Beispiel zeigt wie man eine asynchrone Operation (in diesem Fall einen Aufruf eines Servers) durchführt und das Ergebnis (erfolgreich oder fehlerhaft) verarbeitet.
1. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise die Funktion resolve() einmal mit dem Parameterwert 11 aufgerufen wird (resolve(11)) und anschließend mit dem Parameterwert 22 aufgerufen wird (resolve(22))? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise zuerst resolve(33) aufgerufen wird und anschließend reject(-1) (d.h. Fehlercode -1)? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt.
	- Wenn in einem Executor einer JavaScript Promise die Funktion `resolve()` mehrfach aufgerufen wird, hat dies *Auswirkungen auf das Ergebnis und den Status der Promise.*
	- Wenn `resolve(11)` und anschließend `resolve(22)` aufgerufen werden, **wird die Promise mit dem letzten Wert, der an `resolve()` übergeben wurde, also mit `22`, resolved.** Das bedeutet, dass die `then` Callbacks mit dem Wert `22` aufgerufen werden und die Promise ihren finalen Status "erfüllt" (fulfilled) hat.
	- Wenn hingegen in einem Executor `resolve(33)` aufgerufen wird und anschließend `reject(-1)`, **wird die Promise mit dem Wert `-1` rejected**. Das bedeutet, dass die `catch` Callbacks mit dem Wert `-1` aufgerufen werden und die promise ihren finalen Status "nicht erfüllt" (rejected) hat.
	- Es ist zu beachten, dass eine Promise **nachdem sie einmal resolved oder rejected wurde, nicht mehr geändert werden kann,** d.h. dass die Promise nur einmal resolved oder rejected werden kann und danach ihren Status behalten wird. 
2. Es ist im JavaScript Code auf keine der folgenden alternativen Arten möglich, die Beispiel-Promise zu resolven: 
	``` Javascript
	let my_promise = new Promise( … ); 
	resolve(my_promise); //funktioniert nicht 
	my_promise.resolve(42); // funktioniert auch nicht 
	```
	Erläutern Sie, wie bzw. **von wo aus die Promise denn dann resolved werden kann und warum das Promise Konzept nicht erlaubt, die Promise auf die obigen zwei Arten zu resolven.** Sie können in ihren Erläuterungen auch gerne den oben weggelassenen Teil des Codes (die drei Pünktchen) beispielhaft konkretisieren, wenn dies für ihre Erläuterungen notwendig sein sollte.
	- In JavaScript wird eine Promise durch den _Aufruf der `resolve()` Methode **innerhalb** des Executors der Promise resolved._ Der Executor ist eine Funktion, die beim Erstellen der Promise als Argument übergeben wird und die die asynchrone Operation durchführt. In dieser Funktion kann die `resolve()` Methode aufgerufen werden, um die Promise erfolgreich zu beenden.
	- *Example*

		```JavaScript
		let my_promise = new Promise((resolve, reject) => {
		  setTimeout(() => {
			resolve(42);
		  }, 2000);
		});
		```

1. **Wozu dient die .then() Methode bei JavaScript Promises?** Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .then() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt.
	- Die `then()` Methode bei JavaScript Promises dient dazu, die **Verarbeitung des Ergebnisses einer erfolgreich abgeschlossenen Promise (resolved) zu steuern**. Sie wird auf ein Promise-Objekt angewendet und akzeptiert eine **Callback-Funktion** als Argument, die aufgerufen wird, wenn das Promise **erfolgreich abgeschlossen** wurde.
	- *Example*

		```JavaScript
		function getDataFromServer() {
		  return new Promise((resolve, reject) => {
		    setTimeout(() => {
		      const data = { name: "John", age: 30 };
		      resolve(data);
		    }, 2000);
		  });
		}
		
		getDataFromServer()
		  .then(data => {
		    console.log("Data received:", data);
		    return data.name;
		  })
		  .then(name => console.log("Name:", name))
		  .catch(error => console.error(error.message));
		```
1. **Wozu dient die .catch() Methode bei JavaScript Promises?** Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .catch() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt. Erläutern Sie ferner die Relation zwischen der .catch() Methode von Promises und dem catch in try/catch Blöcken. Wie ist die Relation zwischen diesen beiden Konstrukten im Kontext der Programmierung mit Promises? Sollten beide benutzt werden?
	- Die `catch()` Methode bei JavaScript Promises dient dazu, die **Verarbeitung von Fehlern einer nicht erfolgreich abgeschlossenen Promise (rejected) zu steuern.** Sie wird auf ein Promise-Objekt angewendet und akzeptiert eine **Callback-Funktion** als Argument, die aufgerufen wird, wenn das Promise **nicht erfolgreich abgeschlossen** wurde.
	- *Example*

		```JavaScript
		function getDataFromServer() {
	  return new Promise((resolve, reject) => {
	    setTimeout(() => {
	      reject(new Error("Could not connect to server"));
	    }, 2000);
	  });
	}
	
	getDataFromServer()
	  .then(data => console.log("Data received:", data))
	  .catch(error => console.error(error.message));
		```
	- Die `catch()` Methode von Promises ähnelt dem catch in try/catch Blöcken, da beide dazu dienen, die Verarbeitung von Fehlern zu steuern. Allerdings gibt es einige Unterschiede:
		- `catch()` in try/catch Blöcken wird verwendet, um Fehler, die im synchronen Teil des Codes auftreten, zu behandeln.
		-  `catch()` Methode von Promises wird verwendet, um Fehler, die im asynchronen Teil des Codes auftreten, zu behandeln.
		-   try/catch Blöcke können mehrere catch Blöcke haben, um unterschiedliche Arten von Fehlern zu behandeln, während jede Promise nur eine `catch()` Methode hat.
1. Was versteht man unter der **Promise Absorption**? Erläutern Sie, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt. 
	- Promise Absorption ist ein Mechanismus, der es ermöglicht, **eine Promise in eine andere Promise zu "absorbieren".** Dies bedeutet, dass das Ergebnis einer Promise als Argument für den Executor einer anderen Promise verwendet wird. Dies ermöglicht es, die Ergebnisse von Promises zu kombinieren und zu verknüpfen, um komplexe asynchrone Abläufe zu steuern.
	- *Example*

		```JavaScript
		const promise1 = new Promise((resolve) => {
		  setTimeout(() => resolve(10), 1000);
		});
		const promise2 = new Promise((resolve, reject) => {
		  setTimeout(() => resolve(20), 2000);
		});
		
		promise1.then((result1) => {
		  return promise2.then((result2) => {
		    return result1 + result2;
		  });
		}).then((finalResult) => {
		  console.log(finalResult); // 30
		});
		```
	- In diesem Beispiel wird das Ergebnis der ersten Promise (10) als Argument für den Executor der zweiten Promise verwendet. Das Ergebnis der zweiten Promise (20) wird dann in die finale callback-Funktion übergeben, die das Ergebnis der beiden Promises (10 und 20) addiert und ausgibt.
	- 
1. Was versteht man unter **Promise Chaining**? Erläutern Sie auch hier, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt.
	-  Promise Chaining ist ein Mechanismus, der es ermöglicht, **mehrere Promises in einer Kette aufzurufen und das Ergebnis jeder Promise als Argument für die nächste Promise zu verwenden.** Dies ermöglicht es, komplexe asynchrone Abläufe zu steuern und die Lesbarkeit des Codes zu verbessern, indem die Ergebnisse von Promises über mehrere Schritte hinweg verknüpft werden.
	- *Example*

		```JavaScript
		fetch('url')
		.then(response => response.json())
		.then(data => {
		    // do something with data
		    return data;
		})
		.then(data => {
		    // do something else with data
		    return data;
		})
		.catch(error => console.error(error));
		```
	- In diesem Beispiel werden mehrere Promises in einer Kette aufgerufen. Die erste Promise, `fetch('url')`, lädt Daten von einer URL herunter. Das Ergebnis dieser Promise wird an die nächste Promise `.then(response => response.json())` übergeben, die das Ergebnis in ein JSON-Format konvertiert. Das Ergebnis dieser Promise wird dann an die nächste Promise `.then(data => {...})` übergeben, in der die Daten verarbeitet werden. Dieser Prozess wiederholt sich mit der nächsten then-Methode. Falls ein Fehler auftritt, wird die catch-Methode aufgerufen und der Fehler ausgegeben.
2. Gegeben eine JavaScript Promise, die schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf dieser Promise werde mittels der .then() Methode ein Callback registriert.  
	``` JavaScript
	let promise1 = Promise.resolve(42); 
		promise1.then( result => { 
		console.log("Resultat: " + result); 
	}); 
	console.log("Skript Ende!");
	```
	Wann wird der Callback ausgeführt werden? Geben Sie die relevanten Mechanismen
	des JavaScript Interpreters bzw. der JavaScript Plattform an, die zu diesem
	Verhalten führen.
	- Der Callback, der mittels der .then() Methode auf der Promise registriert wird, **wird sofort ausgeführt, nachdem die Promise erstellt wurde und in den "resolved" Zustand versetzt wurde.** Der Callback wird ausgeführt, bevor die Ausgabe "Skript Ende!" erfolgt.
	- Dieses Verhalten ist auf **mehrere Mechanismen des JavaScript Interpreters zurückzuführen.** Einer davon ist die Implementierung der **Event-Loop** in JavaScript, die sicherstellt, dass Callbacks, die auf erfolgreich abgeschlossene asynchrone Operationen registriert wurden, in der richtigen Reihenfolge ausgeführt werden. Da die Promise bereits im "resolved" Zustand ist, sobald sie erstellt wurde, wird der Callback sofort in die Warteschlange der Event-Loop gestellt und ausgeführt.
	- Ein weiterer Mechanismus ist die Verwendung von **Microtasks und Macrotasks** im JavaScript Interpreter. die Promise.resolve() erzeugt eine Microtask, die von JavaScript Interpreter sofort ausgeführt wird, bevor die nächste Macrotask ausgeführt wird.
34. Erläutern Sie das Konzept der Promise.all() Operation: Wozu wird diese Operation genutzt? Was sind die Parameter und was ist der Rückgabewert dieser Operation? Wie werden Fehlerszenarien durch diese Operation behandelt?
	- Die `Promise.all()` Operation ist eine Methode in JavaScript, die es **ermöglicht, mehrere Promises gleichzeitig auszuführen und auf das Ergebnis aller Promises zu warten, bevor eine Aktion ausgeführt wird.** Sie nimmt als **Parameter ein Array von Promises** und gibt eine einzige Promise zurück, die erfüllt wird, wenn alle Promises im Array erfüllt wurden oder **verworfen wird, wenn mindestens eine der Promises verworfen wurde.**
	- *Example*

		```JavaScript
		const promise1 = fetch('url1');
		const promise2 = fetch('url2');
		const promise3 = fetch('url3');
		
		Promise.all([promise1, promise2, promise3]).then(responses => {
		    //responses is an array of responses from all the promises
		    //process responses
		}).catch(error => console.log(error));
		```
	- In diesem Beispiel werden drei Promises (promise1, promise2, promise3) erstellt und an die `Promise.all()` Methode übergeben. Die Methode führt alle Promises gleichzeitig aus und wartet auf das Ergebnis aller Promises, bevor sie eine Aktion ausführt. Wenn alle Promises erfüllt wurden, wird der Callback in der `then()` Methode aufgerufen und das Ergebnis, welches ein Array von Responses ist, wird verarbeitet. Wenn jedoch mindestens eine der Promises verworfen wird, wird der Callback in der `catch()` Methode aufgerufen und der Fehler wird ausgegeben.

1. Was versteht man unter der "**Promisification**" einer JavaScript Library? Muss dazu die Library immer komplett neu geschrieben werden? Oder gibt es ein alternatives, effizienteres Vorgehen, dass sich bei manchen Libraries umsetzen ließ? Beschreiben Sie mittels JavaScript Pseudo-Code, wie dieses alternative, effiziente Verfahren funktioniert.
	- "Promisification" bezieht sich auf den Prozess, eine JavaScript Library, die **ursprünglich Callbacks verwendet, um auf das Ergebnis asynchroner Operationen zu warten, in eine Version zu konvertieren, die stattdessen Promises verwendet.** Dies ermöglicht es, den Code lesbarer und einfacher zu gestalten, da asynchrone Abläufe mithilfe von Promises einfacher gestaltet werden können.
	- Die komplette Neuschreibung der Library ist nicht immer notwendig, um sie in eine "promisfied" Version zu konvertieren. Es gibt ein alternatives, effizienteres Vorgehen, das sich bei manchen Libraries umsetzen lässt. Dieses Verfahren wird als "**Wrapping**" bezeichnet und besteht darin, die bestehende Library in eine neue Funktion zu "wrappen", die Promises zurückgibt, anstatt Callbacks zu verwenden. 
	- *Example*

		```JavaScript
		//Original Library
		function loadData(callback) {
		    //load data from somewhere
		    //callback(data);
		}
		
		//Wrapped promisified version
		function loadData() {
		  return new Promise((resolve, reject) => {
		    //load data from somewhere
		    if(success) resolve(data);
		    else reject(error);
		  });
		}
		```
	- In diesem Beispiel wird die originale Library `loadData(callback)` in eine neue Funktion `loadData()` gewrappt, die eine Promise zrückgibt. Der Code innerhalb der Funktion bleibt unverändert, aber anstatt einen Callback zu verwenden, wird die Promise mit `resolve(data)` erfüllt, wenn der Datenladen erfolgreich war oder mit `reject(error)`, wenn ein Fehler aufgetreten ist.
	- Dieses Verfahren ermöglicht es, **bestehende Codebasen, die Callbacks verwenden, schrittweise in eine "promisified" Version umzuwandeln, ohne die Notwendigkeit, die gesamte Library neu zu schreiben.** Es ermöglicht auch eine einfachere Fehlerbehandlung und eine bessere Lesbarkeit des Codes, da asynchrone Abläufe mit Promises einfacher gestaltet werden können.
1. Beschreiben Sie anhand eines von Ihnen erdachten illustrativen JavaScript Codebeispiels, was das JavaScript async / await Konstrukt bei einer asynchronen, vom Programmierer definierten Funktion genutzt wird. Wofür wird der await Befehl genutzt, d.h. welche Funktion hat er in der technischen Realisierung der Nebenläufigkeit in JavaScript. Erläutern Sie dies insbesondere an ihrem Codebeispiel. Welche Rolle spielt das async Schlüsselwort für den Code der asynchronen Funktion bzw. für ihre Abarbeitung?
	- Das `async / await` Konstrukt in JavaScript ermöglicht es, **asynchrone Codeabschnitte in einer synchronen Art und Weise zu schreiben.** Es basiert auf **Promises** und ermöglicht es, **asynchrone Operationen mit dem `await` Schlüsselwort zu pausieren und auf das Ergebnis zu warten, bevor der Code fortgesetzt wird.**
	- *Example*

		```JavaScript
		async function getData() {
		  try {
			const response = await fetch('https://example.com/data');
			const data = await response.json();
			console.log(data);
		  } catch (error) {
			console.error(error);
		  }
		}
		```
	- In diesem Beispiel definiert die Funktion `getData()` eine asynchrone Operation, indem das Schlüsselwort `async` verwendet wird. Innerhalb der Funktion wird die `fetch()`-Funktion verwendet, um Daten von einer URL abzurufen. Das `await` Schlüsselwort wird verwendet, um darauf zu warten, dass die Daten geladen werden, bevor sie weiter verarbeitet werden. Das Ergebnis wird in die Variable `response` gespeichert und mittels `response.json()` in ein JSON-Objekt umgewandelt.
	- Das `async` Schlüsselwort ist wichtig, weil es anzeigt, dass die Funktion asynchrone Arbeit enthält und dass das Ergebnis in einer Promise zurückgegeben wird. Der `await` Befehl kann nur innerhalb einer asynchronen Funktion verwendet werden. Mit `await` wird die Ausführung des Codes gestoppt, bis die Promise erfüllt oder verworfen wurde und das Ergebnis des Befehls weitergegeben wird. Dies ermöglicht es, asynchrone Abläufe in einer synchronen Art und Weise zu schreiben und macht den Code lesbarer und einfacher zu verstehen.
1. Beschreiben Sie für das JavaScript async / await Konstrukt, was die async Funktion für einen Rückgabewert besitzt. Erläutern Sie, welche Rolle der Rückgabewert der Funktion und der Rumpf der Funktion im Kontext der Nebenläufigkeit in JavaScript haben.
	- _Wenn eine async Funktion aufgerufen wird, wird der JavaScript Interpreter den Code innerhalb der Funktion ausführen und dann eine neue Promise erstellen. Diese Promise wird in den "pending" Zustand versetzt und wartet darauf, dass die asynchrone Operation abgeschlossen wird. Wenn die asynchrone Operation erfolgreich ist, wird die Promise mit dem Ergebnis erfüllt und das Ergebnis wird über die `resolve()`-Funktion übergeben. Wenn die asynchrone Operation fehls schlägt, wird die Promise mit dem Fehler verworfen und der Fehler wird über die `reject()`-Funktion übergeben._
	- **Der Rückgabewert der async Funktion ist die erstellte Promise.** Mit dieser Promise kann der Aufrufer des Codes auf das Ergebnis der asynchronen Operation warten, indem er die `.then()` Methode verwendet, um einen Callback zu registrieren, der aufgerufen wird, wenn die Promise erfüllt wurde, oder die `.catch()` Methode verwendet, um einen Callback zu registrieren, der aufgerufen wird, wenn die Promise verworfen wurde.
	- In der Praxis ermöglicht das async/await Konstrukt, asynchrone Abläufe in einer synchronen Schreibweise zu gestalten. Mit async/await kann eine asynchrone Funktion durch einen einfachen Aufruf gestartet werden, ohne dass der Aufrufer auf einen Rückruf warten muss, und die Ergebnisse der asynchronen Funktion können einfach mit await abgerufen werden, als ob es sich um eine synchronen Aufruf handeln würde.
2. Erläutern Sie den Vorteil für den Programmierer der JavaScript Programmierung mittels async / await gegenüber einer Programmierung ausschließlich mit Promises (ohne Nutzung von async / await).
	- Das Verwenden von async / await bietet mehrere Vorteile für den Programmierer im Vergleich zur Programmierung ausschließlich mit Promises.
		- Einer der wichtigsten Vorteile ist die Möglichkeit, **asynchrone Abläufe in einer synchronen Schreibweise zu gestalten.** Mit async / await kann der Programmierer die Ergebnisse asynchroner Funktionen einfach mit await abrufen, als ob es sich um synchronen Aufrufe handeln würde, anstatt den Aufruf in einen Callback zu verpacken, was die Lesbarkeit des Codes verbessert und den Programmieraufwand reduziert.
		- **Des Weiteren ermöglicht async/await das Schreiben von sogenannten "try-catch" Blöcke, um Fehler in asynchronen Funktionen zu behandeln, **was die Fehlerbehandlung erleichtert und die Fehlersuche vereinfacht.
		- Ein weiterer Vorteil von async/await ist die Möglichkeit, mehrere asynchrone Aufgaben parallel auszuführen und auf ihre Ergebnisse zu warten, indem man die Funktion `Promise.all()` oder `Promise.race()` verwendet.
	- In der Praxis ermöglicht async/await eine einfachere und lesbarere Programmierung asynchroner Abläufe, und das macht es zu einer beliebten Wahl für JavaScript-Entwickler. 
1. Gegeben eine JavaScript Promise, die innerhalb einer async Funktion definiert wird und schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf diese Promise werde mittels des await Befehls gewartet. 
	```JavaScript
	async function fun_async() { 
		let promise = Promise.resolve(42); 
		let result = await promise; 
		console.log(result); 
		return result; 
	} 
	let promise2 = fun_async(); 
	// ... Wann wird der Code hinter dem await ausgeführt werden? 
	```
	Geben Sie die relevanten Mechanismen des JavaScript Interpreters bzw. der JavaScript Plattform an, die zu diesem Verhalten führen.
	- **Der Code hinter dem await Befehl innerhalb der async Funktion wird ausgeführt, sobald die Promise erfüllt wurde, also sobald der Zustand der Promise auf "resolved" gewechselt ist. Da der Promise bereits per Definition resolved ist, wird der JS Interpreter sofort weiter machen.**
	- Der JavaScript Interpreter führt die async Funktion aus und trifft auf den await Befehl. Anstatt weiter die Ausführung der Funktion fortzusetzen, "pausiert" der Interpreter die Ausführung der Funktion an dieser Stelle und wartet auf die Erfüllung der angegebenen Promise (promise.resolve(42)) . Sobald die Promise erfüllt wird, also ihr Wert zur Verfügung steht, wird die Ausführung der Funktion fortgesetzt und der Wert der Promise wird der Variablen "result" zugewiesen. Der Code nach dem await Befehl wird ausgeführt, und die Funktion gibt den Wert der promise zurück.
1. Erläutern Sie den Vorteil der JavaScript Programmierung mittels des async / await Konstrukts gegenüber einer Programmierung ausschließlich mit Promises, wenn es mehrere asynchrone Operationen gibt, die über Daten voneinander abhängen, d.h. jede Operation benötigt das Ergebnis der Vorgängeroperation. Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.
	- Wenn es mehrere asynchrone Operationen gibt, die über Daten voneinander abhängen, kann die Verwendung von async / await die Programmierung erheblich vereinfachen und die Lesbarkeit des Codes verbessern.
	- Ein Beispiel dafür könnte sein, dass man Daten von einer API abrufen möchte, die sich aus mehreren Schritten zusammensetzen und die Ergebnisse der vorherigen Schritte für die nächsten Schritte benötigen. Ohne async / await könnte die Programmierung schnell unübersichtlich und komplex werden, da man viele verschachtelte Callbacks und Promises verwenden müsste, um die Abhängigkeiten der Schritte zu verwalten.
	- Mit async / await kann man den Code jedoch viel einfacher und lesbarer gestalten, indem man den Ablauf der Schritte mit await-Anweisungen und try-catch-Blöcken organisiert.
	- *Example*

		```JavaScript
		async function fetchData() {
		  try {
		    // Schritt 1: Hole die erste Datenmenge von der API
		    let data1 = await fetch('api/data1');
		    // Schritt 2: Verarbeite die erste Datenmenge
		    let processedData1 = processData(data1);
		    // Schritt 3: Hole die zweite Datenmenge von der API mit processedData1 als Parameter
		    let data2 = await fetch('api/data2?param=' + processedData1);
		    // Schritt 4: Verarbeite die zweite Datenmenge
		    let processedData2 = processData(data2);
		    // Schritt 5: Gebe die verarbeiteten Daten zurück
		    return processedData2;
		  } catch (error) {
		    // Schritt 6: Behandle Fehler
		    console.log("Es ist ein Fehler aufgetreten: " + error);
		  }
		}
		
		let result = await fetchData();
		console.log(result);
		```
	- In diesem Beispiel werden die Schritte der Datenabfrage und -verarbeitung in einer async Funktion organisiert und mit await-Anweisungen gesteuert, um sicherzustellen, dass die nächste Operation erst ausgeführt wird, wenn die vorherige abgeschlossen ist und die benötigten Daten zur Verfügung stehen. Auch die Fehlerbehandlung wird vereinfacht, da man mit try-catch-Blöcken die Fehler innerhalb der Funktion abfangen und behandeln kann.
1. Erläutern Sie das Konzept der Fehlerbehandlung beim JavaScript async / await Konstrukt. Wie ist die Relation zur Fehlerbehandlung bei der Programmierung (ausschließlich) mittels Promises und wie ist die Relation zur Fehlerbehandlung bei sequentiellen (nicht-nebenläufigen) JavaScript Code? Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.
	- Beim JavaScript async / await Konstrukt wird die Fehlerbehandlung über try-catch-Blöcke realisiert. Wenn ein Fehler in einer asynchronen Funktion auftritt, die mit await aufgerufen wird, wird dieser Fehler automatisch von der JavaScript-Engine in den try-catch-Block weitergeleitet.
	- Im Vergleich zur Fehlerbehandlung bei der Programmierung mit Promises, ermöglicht das async / await Konstrukt eine einfachere und lesbarere Fehlerbehandlung, da man die Fehler direkt innerhalb der Funktion, in der sie auftreten, abfangen und behandeln kann, anstatt sie in den Callback-Funktionen der .then() und .catch() Methoden zu behandeln.
	- Im Vergleich zur Fehlerbehandlung bei sequentiellen (nicht-nebenläufigen) JavaScript Code, ermöglicht das async / await Konstrukt eine einfachere Fehlerbehandlung in asynchronen Szenarien, da man die Fehler direkt innerhalb der asynchronen Funktion abfangen und behandeln kann, anstatt sie mit Callbacks oder Promises zu verwalten. 
	- *Example*

		```JavaScript
		async function fetchData() {
		  try {
		    // Schritt 1: Hole die Daten von der API
		    let data = await fetch('api/data');
		    // Schritt 2: Verarbeite die Daten
		    let processedData = processData(data);
		    // Schritt 3: Gebe die verarbeiteten Daten zurück
		    return processedData;
		  } catch (error) {
		    // Schritt 4: Behandle Fehler
		    console.log("Es ist ein Fehler aufgetreten: " + error);
		  }
		}
		
		let result = await fetchData();
		console.log(result);
		```
	- In diesem Beispiel werden die Schritte der Datenabfrage und -verarbeitung in einer async Funktion organisiert und mit await-Anweisungen gesteuert. Wenn ein Fehler in einer der Schritte auftritt, wird er automatisch im catch-Block abgefangen und behandelt. Dies ermöglicht eine einfachere und übersichtlichere Fehlerbehandlung, da alle Fehler in einem zentralen Punkt abgefangen und behandelt werden, anstatt in jedem Schritt einzeln überprüft zu werden.
	- Es ist zu beachten, dass es auch möglich ist, Fehler, die in einer asynchronen Funktion mit await aufgerufen werden, mit einer catch()-Methode zu behandeln. Es ist jedoch die Verwendung von try-catch-Blöcken die empfohlene Methode für die Fehlerbehandlung mit async / await.
1. Erläutern Sie das **Konzept der objekt-basierten / prototypischen Vererbung in JavaScript.** Welche Operation ist die zentrale Operation bei dieser Art der Objekt Orientierung, um das Konzept der Vererbung zu realisieren (es existieren mehrere Standardnamen für diese Operation, geben sie mindestens einen dieser Namen an)? Schreiben Sie JavaScript Code, um folgendes (ausschließlich) mittels dieser Art der Objekt-Orientierung zu realisieren: Alle Shape2D haben eine x und eine y Koordinate und unterstützen die parameterlose Methode mirror(). Der Rumpf der Methode kann leer gelassen werden. Rectangle erbt von Shape2D und bietet zusätzlich die parameterlose Methode area(). Auch hier kann der Methodenrumpf leergelassen werden. Square erbt von rectangle und überschreibt die Methode area(). my_square1 ist ein Square, bei dem sie dann x auf den Wert 10 und y auf den Wert 20 setzen (über direkte zuweisungen an die Attribute).
	- Das Konzept der objekt-basierten / prototypischen Vererbung in JavaScript ermöglicht es, dass ein Objekt von einem anderen Objekt erben kann. _Diese Art der Vererbung basiert auf dem Konzept des Prototyps, bei dem jedes Objekt einen Prototyp hat, von dem es Eigenschaften und Methoden erbt._
	- Die zentrale Operation bei dieser Art der Objekt-Orientierung ist die Verwendung des Schlüsselworts "**prototype**" zum Definieren von Eigenschaften und Methoden, die von einem Objekt geerbt werden sollen. 
	- *Example*

		```JavaScript
		// Define Shape2D with x and y coordinates and mirror() method
		function Shape2D() {
		    this.x = 0;
		    this.y = 0;
		}
		
		Shape2D.prototype.mirror = function() {};
		
		// Define Rectangle that inherits from Shape2D and adds area() method
		function Rectangle() {}
		Rectangle.prototype = Object.create(Shape2D.prototype);
		Rectangle.prototype.constructor = Rectangle;
		Rectangle.prototype.area = function() {};
		
		// Define Square that inherits from Rectangle and overrides area() method
		function Square() {}
		Square.prototype = Object.create(Rectangle.prototype);
		Square.prototype.constructor = Square;
		Square.prototype.area = function() {};
		
		// Create a Square instance and set x and y coordinates
		let my_square1 = new Square();
		my_square1.x = 10;
		my_square1.y = 20;
		```
	- In diesem Beispiel erbt die Klasse `Rectangle` von der Klasse `Shape2D` indem `Rectangle.prototype = Object.create(Shape2D.prototype);` und `Rectangle.prototype.constructor = Rectangle;` genutzt werden. Auf ähnliche Weise erbt die Klasse `Square` von der Klasse `Rectangle`. Ein Objekt `my_square1` wird erstellt und seine x und y Koordinaten werden direkt zugewiesen.
1. Was versteht man unter **prototypischer Vererbung** (auch objekt-basierte Vererbung genannt) in JavaScript? Nennen und beschreiben Sie drei charakteristische Aspekte, die wesentlich in diesem Konzept sind. Dies können z.B. Aspekte sein, wie dieses Konzept „funktioniert“ oder z.B. zentrale Konstrukte der Programmiersprache, die in diesem Konzept relevant sind.
	- Prototypische Vererbung ist ein Konzept in JavaScript, das es ermöglicht, **Objekte auf der Grundlage von anderen Objekten zu erstellen.** Es handelt sich dabei um eine objektbasierte Vererbung, bei der jedes JavaScript-Objekt einen Prototypen hat, von dem es Eigenschaften und Methoden erben kann.
	1.  _Prototypen-Chain_: Jeder Prototyp eines Objekts ist selbst wieder ein Objekt und hat selbst einen Prototypen. Dies bildet eine Kette von Prototypen, die verwendet wird, um die Eigenschaften und Methoden eines Objekts zu suchen. Wenn eine Eigenschaft oder Methode in einem Objekt nicht gefunden wird, wird die Suche im Prototypen des Objekts fortgesetzt und so weiter, bis entweder die Eigenschaft oder Methode gefunden wird oder das Ende der Prototypenkette erreicht ist.
	2. _Konstruktor-Funktionen_: JavaScript hat Konstruktor-Funktionen, die verwendet werden, um neue Objekte zu erstellen, die von einer bestimmten Prototypen-Kette erben. Jede Konstruktor-Funktion hat eine Eigenschaft "prototype", die verwendet wird, um den Prototypen des erstellten Objekts festzulegen.
	3. _Object.create_: JavaScript hat auch die Methode "Object.create", die verwendet werden kann, um neue Objekte zu erstellen, die von einem bestimmten Objekt als Prototypen erben. Dies ermöglicht es, genau zu steuern, von welchem Objekt ein neues Objekt erben soll, anstatt dies durch den Aufruf einer Konstruktor-Funktion zu tun. 
1. Was versteht man unter **pseudo-klassischer Vererbung** in JavaScript? Nennen und beschreiben Sie auch hier drei charakteristische Aspekte, die wesentlich in diesem Konzept sind.
	5. _Objekte als Prototypen_: In JavaScript werden Objekte als Prototypen verwendet, um die Vererbung zu realisieren. J**edes Objekt hat einen Prototypen, der als "Vater-Objekt" fungiert und Eigenschaften und Methoden bereitstellt, die von seinen "Kind-Objekten" geerbt werden können.**
	6.  _Prototypenkette_: In JavaScript bildet **jedes Objekt eine Prototypenkette, die durch Verweise auf den Prototypen des Vater-Objekts und dessen Vater-Objekts etc. aufgebaut wird.** Beim Zugriff auf eine Eigenschaft oder Methode eines Objekts wird die Prototypenkette durchsucht, um diese Eigenschaft oder Methode zu finden.
	7.  _Dynamische Vererbung_: In JavaScript ist die **Vererbung dynamisch und kann zur Laufzeit verändert werden.** Ein Objekt kann z.B. durch Zuweisung eines neuen Prototyps oder durch Hinzufügen oder Ändern von Eigenschaften und Methoden verändert werden, was Auswirkungen auf alle seine Kind-Objekte hat.
2. Gegeben sei folgender JavaScript Code. Zeichnen Sie das Diagramm der Objekte und ihrer Attribut-Beziehungen zueinander, dass sich bei der JavaScript-internen Umsetzung dieses Programmcodes ergibt. Orientieren Sie sich an der in der Vorlesung verwendeten Diagramm-Notation.
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
	- Das Diagramm der Objekte und ihrer Attribut-Beziehungen, die sich bei der JavaScript-internen Umsetzung des oben gegebenen Programmcodes ergibt, würde wie folgt aussehen:
	- *Rectangle*:
		-   length
		-   width
		-   getArea()
		-   toString()
	- *Square* (erbt von *Rectangle*):
		-   length
		-   width
		-   toString()
	- var *rect*:
		-   length: 5
		-   width: 10
	- var *square*:
			-   length: 6
		-   width: 6
	- Der Prototyp des Square-Objekts verweist auf ein Rectangle-Objekt, das die Methoden getArea() und toString() enthält. Der Square-Prototyp überschreibt jedoch die toString() Methode. Beide Objekte, rect und square, haben jeweils ihre eigenen Eigenschaftswerten für length und width.

1. Erläutern Sie, was man bei JavaScript unter dem Begriff des "**Polyfilling**" versteht. (Den Code für das Polyfilling von Object.create() werde ich nicht erfragen. Auch nicht, wie man auf die Existenz testet und dann beim "fehlen" das Polyfilling macht. Aber als Beispiel für Polyfilling sollte man Object.create angeben können.)
	- **Polyfilling bezieht sich auf die Praxis, ältere oder nicht vollständig unterstützte JavaScript-Funktionalitäten in älteren Browsern durch Code-Implementierungen zu ersetzen, die dieselbe Funktionalität bereitstellen.** Dieser Code wird normalerweise als _polyfill_ bezeichnet. Ein Beispiel dafür ist die Verwendung von Object.create(), einer Methode, die in älteren Browsern nicht vorhanden ist, aber in neueren Browsern verfügbar ist. Um diese Methode in älteren Browsern verfügbar zu machen, kann ein polyfill implementiert werden, das eine ähnliche Funktionalität bereitstellt. 