**Donnerstag 2. Februar 2023, 8:30-10:30 Uhr.** 
**Dauer der Klausur: 120 Minuten**

**Wichtig!:**
Diese Präsentation umfasst nicht alle Themen des Semesters aus meinem Teil. Somit gibt es auch nicht zu allen Teilen einen „Beispiel-Fragenkatalog“. Die anderen Themen sind aber genauso prüfungsrelevant: Alle meine Themen aus dem Semester sind komplett prüfungsrelevant! Die Idee der Prüfungsinfos ist ja, Ihnen konkret ein Verständnis zu geben, wonach ich in schriftlichen bzw. mündlichen KMPS Prüfungen frage, d.h. wie sie sich für die Prüfungen vorbereiten müssen / sollten. Dies wird beispielhaft anhand ausgewählter Themen des Semesters konkret gemacht. Dieselbe „Idee der Art zu fragen“ gilt dann auch für alle meine anderen Themen des Semesters!

## Fragen/Aufgaben

Beschreiben Sie, was man unter Concurrency und was man unter Parallelismus versteht. Grenzen Sie die beiden gegeneinander ab. 
	- Concurrency bedeutet, die Möglichkeit mehrere Programmteile unabhängig voneinander in verschiederner Reihenfolge auf einem oder mehreren Prozessor oder Rechner laufen zu lassen. Wenn mehrere Prozessoren verfügbar sind, ist es möglich die Programmteile parelell ausführen. Wenn nur ein Prozessor oder Rechner verfügbar ist, können die Programmteile nur sequentiell ausgeführt werden. Dabei arbeiten die Programmteile trotzdem zusammen, was zur Abhängigkeiten zwischen den Programmteile führen kann. Diese Abhängigkeiten werden gelöst, in dem bei mehreren Prozessoren, einige Programmteile warten ggf. pausiert werden müssen.
	- Parallelismus ist eine Art von Concurrency, wobei die Programmteile zur gleicher Zeit auf mehreren Prozessoren ausgeführt werden können.

Beschreiben Sie das Scheduling nebenläufiger Programmteile in Go und in Erlang und grenzen Sie diese beiden Scheduling Konzepte gegeneinander ab. 
	- Nebenläufigkeit in Go werden mithilfe von Goroutine realisiert, die auf von dem Scheduler auf verfügbare Threads verteilt werden. Die Verwaltung von Goroutinen werden von dem sogenannten Green Thread realisiert. Der ganze Ablauf passiert in dem user space von Goruntime und nicht im kernel space, deswegen ist es OS unabhängig und funktioniert auch auf OS:s ohne Threads Support. Blockierende Goroutine blockieren nicht den Thread, es kommt einfach eine neue Goroutine auf dem Thread dran. Diese können über buffered oder unbuffered Channels miteinander kommunizieren. Der Typ, der übertragenden Wert muss schon bei der Erzeugung der Channel angegeben werden.
	- Nebenläufigkeit in Erlang werden mithilfe von entkoppelten parallelen Erlang Prozesse realisiert, die von dem Erlang VM erzeugt und verwaltet werden. Die Prozesse sind Teil eines Aktoren Modells. Die sind nebenläufige Einheiten, die nicht über geteilten Speicherbereich verfügen, sondern ausschließlich über Nachrichtenaustausch kommunizieren. 

Was versteht man unter kooperativem Scheduling? Nennen Sie dabei auch einen Vorteil und einen Nachteil dieses Scheduling Ansatzes. Beschreiben Sie das kooperative Scheduling in der Programmiersprache Go.
	- Unter kooperativem Scheduling versteht man, dass der Scheduler die Entscheidung selber dem Prozess überlässt, wann er die Kontrolle wieder zurückgibt. In der Regel sind jede Dienst-Anforderungen an das Betriebssystem mit einem Task-Wechsel verbunden
	- Ein Vorteil davon ist, dass das Konzept hilft Race Condition zu vermeiden. Ein Nachteil wäre, dass die Task die Kontrolle nicht schnell genug zurückgeben, das gesamte System blockieren können.
	- Bei der Programmiersprache Go wird auch das Konzept von kooperativem Scheduling umgesetzt, in dem Go Routine selber entscheiden können, wann sie die Kontrolle zurückgeben, wenn ein Go Routine idle oder eben blockiert oder explizit an besonderen Stellen im Programmfluss mittel runtime.GoSched

Wie wird der Wechsel der Kontrolle zwischen den nebenläufigen Goroutinen eines Go Programms veranlasst? Muss der Programmierer des Programms dies immer selbst programmieren?
	- In der Regel sind jede Dienst-Anforderungen an das BS mit einem Task-Wechsel verbunden. In Go wird die Kontrolle kooperative abgegeben, heißt wenn Go Routine idle oder blockiert oder an besonderen Stellen im Programmfluss explizit mittels runtime.GoSched. Wenn eine Go Routine zu lange die Kontrolle nicht zurückgibt, wird von dem Scheduler unterbrochen

Beschreiben Sie die Konzepte der Programmiersprachen Go und Erlang betreffend geteilte Daten zwischen nebenläufigen Programmteilen.
	- In Go heißt das Konzept Channels. Channels sind Kommunikationskanäle für Datenaustuch. Der Typ der zu transportierenden Nachricht muss schon bei der Erzeugung des Channels angegeben werden. Zwei Operationen sind Möglich: Senden und Empfangen. Wenn sich eine Go Routine auf einem Channel anschließt, wird direkt blockiert. Mittels dem select Befehl, kann es aber umgegangen werden. Es wichtig, dass jede Nachricht erfolgreich vermittelt werden können
	- In der Sprache Erlang wird das Konzept von dem Aktorenmodell umgesetzt. Dies ist ein Modell in Informatik für nebenläufige Berechnungen und Programme. Diese werden in nebenläufige Einheiten unterteilt, die ausschließlich über Nachrichtenaustauch kommunizieren. Jede Aktor verfügt über ein Posteingang, eine Adresse und ein Verhalten.
	- Der Empfang einer Nachricht wird als Ereignis bezeichnet und dies wird im Posteingang gespeichert. Das Verhalten des Aktores beschreibt die Reaktion auf eine Nachricht abhängig von deren Aufbau. Es gibt 3 verschiedenen Reaktionen: 1. die Nachricht an sich selbst oder an einem anderen Aktor zu senden 2. neue Aktoren zu erzeugen 3. eigenes Verhalten ändern. Das Aktoren Model legt nicht fest, wie lange die Vermittlung einer Nachricht dauern darf. Es ist nur definiert, dass jede Nachricht nach einer endlichen Zeit bei dem Empfänger an kommen muss. Der Nachrichtenaustausch erfolgt asynchron, d.h der Sender kann nach dem Absenden sofort mit einer anderen Aktion weitermachen und muss nich auf Bestätigung des Empfängers warten.

Nennen Sie den wesentlichen Unterschied zwischen Goroutinen und den Threads des zugrundeliegenden Betriebssystems, der dazu führt, dass Goroutinen nicht 1:1 auf Threads abgebildet werden.
	- Goroutine sind viel leichtgewichtiger als Threads. Ein Thread hat 1Mb Stack size, eine Goroutine nur 2Kb. Threads nutzen preemptives Scheduling, deswegen müssen viele Register gesichert werden, was zu einem teueren Context Switch führt. Goroutine verwenden hingegen kooperatives Scheduling in Go Runtime, was leichtgewichtigtere Context Switch ergibt. Goroutine werden vom dem Go Runtime verwaltet, Scheduling durch den sogenannten Green Threads. Dies passiert im user space von Go Runtime und nicht im kernel space. Dies führt dazu dass Goroutine auch auf OS:s ohne Thread Support funktionieren. 

Was versteht man in Go unter einem Tight Loop Szenario? Geben Sie ein Beispiel (Go Codeausschnitt) dafür an. Wie kann der Programmierer dieses Problem lösen?
	- Tight Loop beschreibt, dass eine Go Routine in einer sehr langen Schleife oder in einer Endloschleife läuft und dabei die Kontrolle nicht abgibt, heißt es werden keine Library Funktionen aufgerufen noch runtime.GoSched. Das heißt, in der Schleife werden nur elementare Operationen, also elementare Berechnungen durchgeführt. Der Programmieren kann Mechanismen in den Code abbauen, die die Schleife expizit abrechen (z.B time.Sleep(), oder mindesten die Kontrolle mittel runtime.GoSched abgibt.
```GO 
var a = true
while(a) {
	const b = 2
}
```
	
Beschreiben Sie, wofür Channels in Go genutzt werden. Welche Arten von Channels gibt es? Für welche Kommunikationsszenarien werden diese genutzt? Mittels welcher Operationen können die Goroutinen solche Channels nutzen? 
	- Channels sind Kommunikationskanäle, an denen sich Goroutine anschließen können. Mithilfe von Channels können Goroutine miteinander Nachrichten auszutauschen. Datentyp der zu übertragenden Werte müssen bei der Erzeugung angegeben werden. Es sind 2 verschiede Arten von Channels: Buffered und unbuffered Channels. Die ledigliche Unterscheid besteht zwichen den beiden dadrin, dass buffered Channels die Werte nicht nur übertragen, sondern auch speichern können. Es gibt 2 verschiedene Operationen bei Channels: Send und Receive. Für verschiedene Kommunikationsszenarien können Channels verwendet werden. Z.B für synchronisierten Beenden von main oder überall, wo wichtig ist mitzuteilen, dass z.B eine Goroutine mit seiner Berechnung durch ist.

Wie verhalten sich die grundlegenden send und receive Operationen bei Unbuffered Channels? 
	- Receive Operation: blockiert solange, bis eine Send Operation auf dem Channel ausgeführt wird, heißt ein Wert gesendet.
	- Send Operation: blockiert solange, bis eine Receive Operation empfangsbereit gemeldet hat. Sobald eine Receive Operation sich auf dem Channel gemeldet hat, wird die Send Operation fortgesetzt und der Wert überreicht.

Wie kann man Unbuffered Channels nutzen, was den Kontrollfluss von Goroutinen betrifft?
	- wesentliche Technik zur Synchronization
	- Unbuffered Channels können in Go verwendet werden, um den Kontrollfluss von Goroutinen zu steuern. Durch das Blockieren von Send- und Receive-Operationen können Sie sicherstellen, dass bestimmte Aktionen in der richtigen Reihenfolge ausgeführt werden und dass bestimmte Goroutinen auf bestimmte Ereignisse warten, bevor sie fortfahren.

Beschreiben Sie, welche Varianten des select Befehls es in Go gibt. Es ist nicht die exakte Syntax dieser Befehlsvarianten gefragt, sondern eine Beschreibung des Konzepts (der Semantik) der Varianten.
	- Recieve: 
		- select blockiert, bis auf einem der Channel eine Nachricht eintrifft
		- es ist auch mit timeout zu realisieren, um nicht zu lange zu blockieren
		- es ist auch mit default case zu realisieren, um auf einem Channel gar nicht zu blockieren
	- Send:
		- select blockiert, bis auf einem der Channel eine Nachricht versendet werden kann
		- es ist auch mit timeout zu realisieren, damit das select nicht zu lange blockiert
		- es ist auch mit default case zu realisieren, um zu vermeiden, dass select auf einem Channel blockiert wo noch keine Empfänger lauscht

Nennen Sie den wesentlichen Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems, der dazu führt, dass Erlang Prozesse nicht 1:1 auf Threads abgebildet werden.
	- Erlang Prozesse sind leicht-gewichtigt und voneinander völlig isoliert, heißt kein Shared Date, keine Seiteneffekte. Die Prozesse werden in dem Erlang VM erzeugt und verwaltet. Erlang Prozesse kommunizieren ausschließlich über Nachrichtenaustausch. Threads können auf gemeinsam genutzte Daten zugreifen, was zu Race Conditions und Deadlock führen kann.

Nennen Sie den Namen des Nebenläufigkeits-Konzepts von Erlang und beschreiben Sie es
	- Das Nebensläufigkeitskozept von Erlang heißt Aktoren Modell. Dies ist ein Modell in Informatik für nebenläufige Berechnungen und Programme. Diese werden in nebenläufige Aktoren unterteilt, die ausschließlich über Nachrichtenaustausch kommunizieren und nich über ein geteilte Speicherbereich verfügen. Ein Aktor hat aber ein Posteingang, eine Adresse und ein Verhalten. Das Verhalten eines Aktors beschreibt wie der Aktor die Reaktion auf den Empfang einer Nachricht (Ereignis). Es existieren 3 verschiedene Rekation: 
		- a. Nachricht an sich selbst oder an einem anderem Aktor weitersenden
		- b. neue Aktore erzeugen
		- c. eigenes Verhalten verändern
	- Der Nachrichtenaustuch erfolgt asynchron, d.h der Sender kann nach dem Absender direkt mit einer anderen Nachricht forsetzen, er muss nicht warten, bis der Empfänger die Nahricht akzeptiert. Das Aktoren Modell legt nicht fest wie lange die Vermittlung einer Nachricht dauern darf. Es ist nur definiert, dass jede Nachricht nach einer bestimmten Zeit bei dem Empfänger ankommen muss. 

Begründen Sie, warum sich Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen lassen, ohne dass dies im Erlang Programm besonders berücksichtigt werden muss. #wiederholen
	- Erlang Prozesse können sich über mehrere im Netzwerk verbundene Rechner verteilen, weil Erlang eine verteilte Programmierumgebung ist. Erlang Prozesse sind unabhängig voneinander und kommunizieren über Nachrichten, was bedeutet, dass sie sich nicht gegenseitig beeinflussen. Dies ermöglicht es, Erlang Prozesse auf verschiedenen Rechnern auszuführen und sie über das Netzwerk miteinander zu kommunizieren, ohne dass das Programm angepasst werden muss. Erlang hat auch eine interne Unterstützung für die Verteilung von Prozessen über mehrere Rechner, die als "distributed Erlang" bezeichnet wird. Dies ermöglicht es Erlang-Programmen, problemlos in einer verteilten Umgebung ausgeführt zu werden, ohne dass das Programm angepasst werden muss.

 In Erlang möchte eine Funktion, die als Client agiert, einer anderen Funktion, die als Server agiert, einen Request (eine Anfrage, als Nachricht) schicken. Nennen Sie zwei Möglichkeiten, wie der Client den Server als "Nachrichtenziel" finden bzw. identifizieren kann, so dass die Nachricht dann beim richtigen Empfänger ankommt.
	 - Durch das globale Registry können Clients die ID von dem Server herausfinden, wo Servers über ihren Namen zu identifizieren sind. (PID)
	 - Server sind unter einem Host über ihren Namen eindeutig zu identifizieren. (Name)

In einem Erlang Programm agiere eine nebenläufige Funktion als Client und eine andere nebenläufige Funktion als Server. Der Client schickt mehrere AnfrageNachrichten an den Server und erhält später mehrere Antwort-Nachrichten. Wie kann der Erlang Programmierer sicherstellen, dass der Client Code die empfangenen Nachrichten der richtigen entsprechenden (vorher geschickten) Anfrage zuordnen kann? #wiederholen
	- Jede Nachricht kann mit einer ID: Der Client kann jeder Anfragenachricht eine eindeutige Nachrichten-ID hinzufügen, die er in jeder Antwortnachricht wiederfindet.
	- Verwenden Sie ein Korrelations-Token: Der Client kann ein eindeutiges Korrelations-Token zu jeder Anfragenachricht hinzufügen und erwartet, das gleiche Korrelations-Token in jeder Antwortnachricht wiederzufinden, um sicherzustellen, dass die Antwortnachricht zur entsprechenden Anfragenachricht gehört.

Beschreiben Sie die Best Practice Empfehlung, wie in Erlang Code mit Fehlersituationen umgegangen werden soll.
	- Den Programmcode in 2 Teilen aufzuteilen: ein Teil, der die Aufgaben löst und ein Teil der die Fehler behebt. Der Teil, der die Aufgaben löst, soll so wenig defensivem Code, wie möglich geschrieben werden. Der Teil, der die Fehler behebt, ist oft generisch, um die gleiche Fehlercode nicht mehrmals implementieren zu müssen. Dies führt zu einer suaberen Trennung der Themen und Verringerung des Codevolumes.

Bei der Erlang Programmierung gilt die Empfehlung: "Let it crash". Beschreiben Sie, wie im Erlang Code trotz crashender Prozesse die Fehlersituation eingegrenzt und strukturiert behandelt werden kann, sodass es nicht zu einem Crash des gesamten Applikationscodes kommen muss.
	- Verschiedene Möglichkeiten stehen zur Verfügung bei Fehlersituation nicht zu einem Crash des gesamtem Applikationcodes kommen muss:
		- Supervisor: Überwachen von ihnen gestarteten Kindprozesse. Supervisor-Strategie gibt an, wie es an bestimmten Stellen bezüglich der Restart-Strategie zu verfahren ist, wenn ein Kind-Prozess stirbt.
		- Bei parallelen Prozessen können die Prozesse mittels eine bidirektionali Link verbunden werden. Wenn ein Prozess stirbt, werden alle gelinkte Prozesse neugestartet.
		- Prozessor Monitoring durch andere Prozesse: Es ist eine unidirektionale Verbindung:  Falls der gemonitorte Prozess stirbt, wird der Monitoring Prozessor benachrichtigt, aber nicht umgekehrt.

Was versteht man im Erlang OTP Framework unter einem Supervisor Prozess und einem Worker Prozess, wenn es um die Behandlung von Fehlersituationen im Erlang Code geht? Welche Rolle spielen die beiden im Kontext der in Erlang programmierten Applikation?
- Supervisor:
	- können Kindprozesse zu starten, zu stoppen und zu monitoren
	- überwachen Workers und / oder andere Supervisors, dadurch entsteht eine baumartige Struktur.
	- Die Supervisor-Strategie gibt an, wie es an bestimmten Stellen in Baum bezüglich der Restart-Strategie zu verfahren ist, wenn ein Kindprozess stirbt.
- Workers:
	- sind für die allgemeine Berechnung verantwortlich, ohne aufwändige Fehlerbehandlung

Wofür benutzt das Erlang OTP Framework die Restart-Strategien? Beschreiben Sie zwei verschiedene dieser Strategien, inklusive Beschreibung eines von Ihnen erdachten Anwendungsfalls, in dem die jeweilige Strategie angemessen ist. #wiederholen #folienanschauen 
	- Die Restart-Strategien werden für Fehlerbehandlung benutzt.
	- simple_one_for_one:
		- Ein Supervisor sitzt einfach nur da und weißt, dass er nur eine Art von Children erzeugen kann.
		- Wenn man ein Kindprozess braucht, fragt man danach und bekommt es.
	- rest_for_all:
		- Wenn ein Prozess stirbt, werden alle nach ihm gestarteten (also von ihm abhängigen) Prozesse auch sterben

Bei Erlang Applikationen, die auf das Framework OTP aufgebaut sind, besteht die Möglichkeit, bei Teilen der Applikation zur Laufzeit Code auszutauschen. Beschreiben Sie, welche Eigenschaften Erlang als Sprache besitzt, auf die diese Möglichkeiten dann aufbauen können.
#wiederholen #folienanschauen 

JavaScript Interpreter bieten im Prinzip nur eine "Ausführungseinheit" für den JavaScript Code und nur eine Event Loop. Trotzdem ist es möglich, nebenläufige Programmteile zu programmieren.
	1. Beschreiben Sie einen Mechanismus, durch den bestimmt wird, ob und wann die nebenläufigen Programmteile zur Ausführung kommen. 
	2. Zu welchen negativen Konsequenzen können die Einschränkungen "eine Ausführungseinheit" und "eine Event Loop" trotz des Vorhandenseins von Nebenläufigkeit führen, wenn man ausschließlich die Ausführung von JavaScript Code betrachtet? Denken Sie sich ein Beispiel aus, welches diesen Effekt illustriert, und beschreiben Sie es high-level.

Was versteht man unter dem JavaScript Callback Programmierstil? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das Callback Prinzip illustriert, und erläutern Sie ihr Beispiel #wiederholen 
- Callback-Programmierung ist ein Konzept der asynchronen Programmierung, bei dem eine Funktion (der "Callback") als Argument an eine andere Funktion übergeben wird, die später aufgerufen wird. Dies ermöglicht es, dass eine Funktion ihre Arbeit fortsetzen kann, während eine andere Funktion ausgeführt wird.
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

 Wie kann man bei der JavaScript Programmierung mittels einfacher Callbacks die Fehlerbehandlung programmieren? Geben Sie ein JavaScript Codebeispiel mit einer von ihnen erdachten JavaScript Callback Funktion an, welches illustriert, wie die Fehlerinformationen einer asynchronen Operation an ein Callback übergeben und von diesem verarbeitet werden könnten. Das Beispiel soll auch die Registrierung der Callback Funktion beinhalten.
	- In JavaScript kann man Fehler in Callbacks durch die Übergabe eines Error-Objekts als erstes Argument des Callbacks handhaben.
	-  In diesem Beispiel wird das Ergebnis des Serveraufrufs simuliert. Wenn der Aufruf erfolgreich ist, werden die Daten an den Callback übergeben, ansonsten wird ein Error-Objekt übergeben. Der Callback prüft dann, ob ein Fehler vorliegt und handelt entsprechend. Wenn es ein Fehler ist, wird die Fehlermeldung auf der Konsole ausgegeben, andernfalls werden die Daten ausgegeben
	- *Example*
 ```javascript 
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

Was versteht man bei der JavaScript Programmierung unter dem Problem der "Callback Hell" (oder "Pyramid of Doom")? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das "Callback Hell" Problem illustriert, und erläutern Sie ihr Beispiel.
	- Programmierung der Nebenläufigkeit asynchroner Programmteile führt bei nacheinander auszuführenden (voneinander abhängigen) Programmteile zu einem tief verschachtelten, unübersichtlichen Callbacks-Programmcode
```javascript 
function getUserData(userId, callback) {
  fetch('https://jsonplaceholder.typicode.com/users/' + userId)
    .then(response => response.json())
    .then(userData => {
      fetch('https://jsonplaceholder.typicode.com/posts?userId=' + userId)
        .then(response => response.json())
        .then(posts => {
          userData.posts = posts;
          fetch('https://jsonplaceholder.typicode.com/comments?postId=' + userData.posts[0].id)
            .then(response => response.json())
            .then(comments => {
              userData.posts[0].comments = comments;
              callback(userData);
            });
        });
    });
}

getUserData(1, (userData) => {
  console.log(userData);
});

```

Erläutern Sie anhand eines von Ihnen erdachten JavaScript Pseudo-Code-Beispiels das Konzept des "Reactive Programming". Erläutern Sie insbesondere, auf welcher Art von Datenstrukturen dieser Ansatz basiert und welche Art von Operationen auf diesen Daten benutzt werden. Beschreiben Sie, welchen Vorteil dieser Programmieransatz bietet gegenüber der klassischen event-basierten Programmierung in JavaScript. #wiederholen 
	- die Reactive Programming benutzt ganze Event Streams als elementare Datenstrukturen. Außerdem werden verschiedene Operationen, wie map oder filter im Kobination mit Event Streams verwendet. 
	- Der Hauptvorteil der reaktiven Programmierung gegenüber der traditionellen ereignisbasierten Programmierung in JavaScript besteht darin, dass sie einen deklarativen und funktionalen Ansatz für die Programmierung ermöglicht. Außerdem ist es einfacher, reaktionsschnelle und hochgradig interaktive Benutzeroberflächen zu erstellen, und es ist einfacher, den Datenfluss in einer Anwendung zu verstehen.
 ```javascript 
const data = {
  name: "John Doe",
  age: 30
};

const dataObserver = new Observer(data, (newData) => {
  console.log(`Data has changed: ${JSON.stringify(newData)}`);
});

dataObserver.name = "Jane Doe";
// Output: Data has changed: {"name":"Jane Doe","age":30}

dataObserver.age = 35;
// Output: Data has changed: {"name":"Jane Doe","age":35}
 ```

Was versteht man bei der JavaScript Programmierung unter dem Konzept der Promises? Was ist eine Promise? Wie wird sie im JavaScript Code programmiert? Geben Sie ein von Ihnen erdachtes JavaScript Pseudo-Code-Beispiel an, in dem eine von Ihnen erdachte JavaScript Funktion eine Promise erzeugt und zurückgibt. Erläutern Sie den Zweck der Promise mittels ihres Beispiels. Erläutern Sie auch, aus welchen Codekonstrukten sich die Promise zusammensetzt und wie diese Konstrukte in ihrem Beispiel genutzt werden, damit die Promise ihren Zweck erfüllen kann.
	- Promise ist ein Konzept für die Realisierung asynchriner Berechnungen
	- Ein Promise ist ein Platzhalteobjekt für einen berechneten Wert, wobei die Berechnung noch nicht vollständig berechnet sein muss.
	- Dieses Objekt kann von dem folgenden, von dem Wert abhängige Programmteil direkt entgegengenommen werden.
	- Wenn die asynchrone Operation fertig berechnet wurde, kann der vom dem Promise abhängigen Programmteil zur Ausführung kommen.
	- Eine Promise hat eine Executor Function, die aus 2 Funktionen, resolve und rejected besteht. Beide nehmen einen Wert als Parameter, und tragen diesen als positives (resolve) bzw. als Fehlerergebniss (rejected) in das Promise Objekt ein. Zustand der Promise ist pending solange, bis der Wert nicht fertig berechnet wurde. Resolve und Reject ändern den Status der Promise auf settled. Wenn sich eine Promise im Zustand settled befindet, werden alle weitere resolve und reject Aufrufe ignoriert. Nur der Rumpf der Executor muss von dem Programmieren explizit programmiert werden, inkl. der dortigen Aufruf von resolve und reject.

Was passiert bei einer JavaScript Promise, wenn im Executor der Promise die Funktion resolve() einmal mit dem Parameterwert 11 aufgerufen wird (resolve(11)) und anschließend mit dem Parameterwert 22 aufgerufen wird (resolve(22))? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise zuerst resolve(33) aufgerufen wird und anschließend reject(-1) (d.h. Fehlercode -1)? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt.
	- Resolve und auch reject setzen den Status der Promise auf settled. Wenn sich die Promise einmal in den Status settled gesetzt wurde, werden alle weitere Aufrufe von resolve und reject ignoriert. Ein späteren Aufruf von Reject kann z.B nicht resolve Wert korrigieren oder umgekehrt.

Es ist im JavaScript Code auf keine der folgenden alternativen Arten möglich, die Beispiel-Promise zu resolven. Erläutern Sie, wie bzw. von wo aus die Promise denn dann resolved werden kann und warum das Promise Konzept nicht erlaubt, die Promise auf die obigen zwei Arten zu resolven. Sie können in ihren Erläuterungen auch gerne den oben weggelassenen Teil des Codes (die drei Pünktchen) beispielhaft konkretisieren, wenn dies für ihre Erläuterungen notwendig sein sollte.
``` Javascript
	let my_promise = new Promise( … ); 
	resolve(my_promise); //funktioniert nicht 
	my_promise.resolve(42); // funktioniert auch nicht 
```
	- Ein Promise kann entweder in der Executor Function resolved werden, mittels resolve() Funktion, oder wenn eine Promise sofort einen resolved Wert erhalten soll, dann mithilfe von Promise.resolve. Diese helfen dabei, die Werte nicht über Seiteneffekte zu erzielen und diese können dann auch nicht von der Außenwelt geändert werden, sind also in der Executor eingeschlossen.

Wozu dient die .then() Methode bei JavaScript Promises? Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .then() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt.
	- .then() Methode ist für den daten-abhängigen Programmteil, wird immer verzögert ausgeführt, auch wenn die Promise schon seinen konkreten Wert hat. Die .then() Methode hat ein Parameter, dieser ist eine Callback-Funktion. 
	- .then Methode gibt immer eine Promise zurück. Dies ermöglicht die Verkettung der Methoden
	- Die Callback-Funktion erwartet einen Parameter, der keine Promise ist, also es ist der Resultatwert der Promise. Dieser Wert für die .then Methode quasi vorher automatisch ausgepackt. Der Rückgabewert wird daher automatisch wieder in eine Promise gepackt.

Wozu dient die .catch() Methode bei JavaScript Promises? Erläutern Sie dies anhand eines von ihnen erdachten illustrativen JavaScript Codebeispiels. Erläutern Sie insbesondere auch, ob die Methode Parameter hat, wenn ja wieviele und wie diese genutzt werden, ob die Methode einen Rückgabewert hat und wenn ja, welche Funktion dieser erfüllt und welche Aufgabe die .catch() Methode im Konzept der Nebenläufigkeit mittels Promises erfüllt. Erläutern Sie ferner die Relation zwischen der .catch() Methode von Promises und dem catch in try/catch Blöcken. Wie ist die Relation zwischen diesen beiden Konstrukten im Kontext der Programmierung mit Promises? Sollten beide benutzt werden?
	- .catch() Methode ist für die einheitliche Error-Handling zuständing, egal ob der Fehler durch reject oder durch ein Exception in .then Rumpf entstanden ist.
	- die Methode hat ein Parameter, eine Callback-Funktion. Diese Funktionen erwartet einen Wert als Parameter. Da die Methode auf eine Promise aufgerufen wird und die vorherige Berechnung eine Promise liefert, muss der Ergebniswert also ausgepackt werden. Als Rückgabewert ist auch eine Promise, und muss wieder eingepackt werden, wenn es keine Promise ist. 
	- Mittels catch kann die Fehlerbehandlung sehr einheitlich gestaltet werden und ist keine weiter try/catch Block notwendig. Ferner ist die Verwendung von try/catch Blöcke für synchrone Fehlerhandlung gedacht und die catch Methode bei einer Promise soll die asynchrone Fehlerhandlung erledigen.
Was versteht man unter der Promise Absorption? Erläutern Sie, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt. Was versteht man unter Promise Chaining? Erläutern Sie auch hier, wie dieser Mechanismus funktioniert und welchen Zweck er für die Programmierung mittels Promises erfüllt. #wiederholen 
	- .then() und .catch() Methoden werden auf einer Promise aufgerufen, aber die Parameterfunktionen der beiden Methoden einen konkreten Wert erwarten. Da die vorherige Berechnung auch eine Promise liefert, müssen die quasi ausgepackt werden und so der Methode übergeben werden. Bei der Rückgabewert, wenn dieser keine Promise ist, muss er wieder in eine Promise eingepackt werden.
	- Da die Methoden immer eine Promise zurückgeben, ist es möglich diese Methoden zu verketten(chainen), also mehrere .then() hintereinander aufzurufen

Gegeben eine JavaScript Promise, die schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf dieser Promise werde mittels der .then() Methode ein Callback registriert.  
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
	- Da die resolve Methode bei der Definition aufgerufen wird, hat die Promise sofort einen Ergebniswert. Ferner wird die Methode resolve auf das ScriptJobQueue von JS Runtime gepusht. Bei der Ausführung dieser Job, wird ein .then() Script auf dem PromiseJobQueue angehängt. Da diese Queue vollständig abgearbeitet werden muss, bevor der nächste Script ausgeführt werden kann, wird zuert der Resultat geprintet. Danach wird weiter ScriptJobsQueue abgearbeitet und Skript Ende geprintet.
Erläutern Sie das Konzept der Promise.all() Operation: Wozu wird diese Operation genutzt? Was sind die Parameter und was ist der Rückgabewert dieser Operation? Wie werden Fehlerszenarien durch diese Operation behandelt?
	- Dabei wird auf das Settlement mehrerer Promise gewartet. Die Parameter dieser Funktion ist ein Array an Promises. Bei asynchronen Plattform-Operationen ist die parallele Ausführung möglich, um die Perfomance zu verbessern. Wenn eine Promise rejected ist, werden alle andere Ergebnisse auch verworfen.
Was versteht man unter der "Promisification" einer JavaScript Library? Muss dazu die Library immer komplett neu geschrieben werden? Oder gibt es ein alternatives, effizienteres Vorgehen, dass sich bei manchen Libraries umsetzen ließ? Beschreiben Sie mittels JavaScript Pseudo-Code, wie dieses alternative, effiziente Verfahren funktioniert.
	- Promisifacition bedeutet die Umwandlung einer Library mit API Funktionen in Callback-Style in einer Variante in Promise-Style. Für eine systematische API der Library kann die Promisification der Library sehr systematisch gemacht werden, mittels einer generellen Wrappen Funktion promisify().

Beschreiben Sie anhand eines von Ihnen erdachten illustrativen JavaScript Codebeispiels, was das JavaScript async / await Konstrukt bei einer asynchronen, vom Programmierer definierten Funktion genutzt wird. Wofür wird der await Befehl genutzt, d.h. welche Funktion hat er in der technischen Realisierung der Nebenläufigkeit in JavaScript. Erläutern Sie dies insbesondere an ihrem Codebeispiel. Welche Rolle spielt das async Schlüsselwort für den Code der asynchronen Funktion bzw. für ihre Abarbeitung?
	- async gibt immer eine Promise zurück, also async = new Promise. Ihr Rump ist also der Executor der Promise und der Rückgabewert der Funktion ist der resolved Wert der Promise.
	- in dem async Rumpf kann mittels await auf einem asynchronen Operatioen gewartet werden
	- await wartet auf das Settlemant deer Promise, d.h die Zeilen hinter dem await sind die Parameter-Funktion eines .then()
	- await ermöglicht daten-abhängigen Progtammteil sequentiell hinter dem await zu implementieren, statt in einer Callback-Funktion eines .thens(). Dabei wird die Kontrolle abgegeben und getrigget, wenn die Promise in seinem konkreten Wert resolved wird. Await kann nur mit einem async oder auf dem top level von einem Modul verwendet werden. 

Beschreiben Sie für das JavaScript async / await Konstrukt, was die async Funktion für einen Rückgabewert besitzt. Erläutern Sie, welche Rolle der Rückgabewert der Funktion und der Rumpf der Funktion im Kontext der Nebenläufigkeit in JavaScript haben.
	- async gibt immer eine Promise zurück, also async ist new Promise()
	- ihr Rumpf ist also der Executor der neuen Promise
	- mittels await kann auf das Settlement der Promise gewartet werden, d.h die Zeilen hinter einem await sind die Parameter-Funktion eines .then()
	- der Rückgabewert ist also der resolved Wert der Promise

Erläutern Sie den Vorteil für den Programmierer der JavaScript Programmierung mittels async / await gegenüber einer Programmierung ausschließlich mit Promises (ohne Nutzung von async / await). #wiederholen 
	- Das Verwenden von async / await bietet mehrere Vorteile für den Programmierer im Vergleich zur Programmierung ausschließlich mit Promises.
	- Einer der wichtigsten Vorteile ist die Möglichkeit, asynchrone Abläufe in einer synchronen Schreibweise zu gestalten. Mit async / await kann der Programmierer die Ergebnisse asynchroner Funktionen einfach mit await abrufen, als ob es sich um synchronen Aufrufe handeln würde, anstatt den Aufruf in einen Callback zu verpacken, was die Lesbarkeit des Codes verbessert und den Programmieraufwand reduziert.
	- Des Weiteren ermöglicht async/await das Schreiben von sogenannten "try-catch" Blöcke, um Fehler in asynchronen Funktionen zu behandeln, was die Fehlerbehandlung erleichtert und die Fehlersuche vereinfacht.
	- Ein weiterer Vorteil von async/await ist die Möglichkeit, mehrere asynchrone Aufgaben parallel auszuführen und auf ihre Ergebnisse zu warten, indem man die Funktion `Promise.all()` oder `Promise.race()` verwendet.
	- In der Praxis ermöglicht async/await eine einfachere und lesbarere Programmierung asynchroner Abläufe, und das macht es zu einer beliebten Wahl für JavaScript-Entwickler.

Gegeben eine JavaScript Promise, die innerhalb einer async Funktion definiert wird und schon bei ihrer Definition in den "resolved" Zustand versetzt wird. Auf diese Promise werde mittels des await Befehls gewartet. async function fun_async() { let promise = Promise.resolve(42); let result = await promise; console.log(result); return result; } let promise2 = fun_async(); // ... Wann wird der Code hinter dem await ausgeführt werden? Geben Sie die relevanten Mechanismen des JavaScript Interpreters bzw. der JavaScript Plattform an, die zu diesem Verhalten führen.
	-  Der Code hinter dem await Befehl innerhalb der async Funktion wird ausgeführt, sobald die Promise erfüllt wurde, also sobald der Zustand der Promise auf "resolved" gewechselt ist.
	- Der JavaScript Interpreter führt die async Funktion aus und trifft auf den await Befehl. Anstatt weiter die Ausführung der Funktion fortzusetzen, "pausiert" der Interpreter die Ausführung der Funktion an dieser Stelle und wartet auf die Erfüllung der angegebenen Promise (promise.resolve(42)) . Sobald die Promise erfüllt wird, also ihr Wert zur Verfügung steht, wird die Ausführung der Funktion fortgesetzt und der Wert der Promise wird der Variablen "result" zugewiesen. Der Code nach dem await Befehl wird ausgeführt, und die Funktion gibt den Wert der promise zurück.

Erläutern Sie den Vorteil der JavaScript Programmierung mittels des async / await Konstrukts gegenüber einer Programmierung ausschließlich mit Promises, wenn es mehrere asynchrone Operationen gibt, die über Daten voneinander abhängen, d.h. jede Operation benötigt das Ergebnis der Vorgängeroperation. Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.
	- Wenn es mehrere asynchrone Operationen gibt, die über Daten voneinander abhängen, kann die Verwendung von async / await die Programmierung erheblich vereinfachen und die Lesbarkeit des Codes verbessern.
	- Ein Beispiel dafür könnte sein, dass man Daten von einer API abrufen möchte, die sich aus mehreren Schritten zusammensetzen und die Ergebnisse der vorherigen Schritte für die nächsten Schritte benötigen. Ohne async / await könnte die Programmierung schnell unübersichtlich und komplex werden, da man viele verschachtelte Callbacks und Promises verwenden müsste, um die Abhängigkeiten der Schritte zu verwalten.
	- Mit async / await kann man den Code jedoch viel einfacher und lesbarer gestalten, indem man den Ablauf der Schritte mit await-Anweisungen und try-catch-Blöcken organisiert.

Erläutern Sie das Konzept der Fehlerbehandlung beim JavaScript async / await Konstrukt. Wie ist die Relation zur Fehlerbehandlung bei der Programmierung (ausschließlich) mittels Promises und wie ist die Relation zur Fehlerbehandlung bei sequentiellen (nicht-nebenläufigen) JavaScript Code? Erläutern Sie dies anhand eines von ihnen erdachten, illustrativen JavaScript Codebeispiels.

Erläutern Sie das Konzept der objekt-basierten / prototypischen Vererbung in JavaScript. Welche Operation ist die zentrale Operation bei dieser Art der ObjektOrientierung, um das Konzept der Vererbung zu realisieren (es existieren mehrere Standardnamen für diese Operation, geben sie mindestens einen dieser Namen an)? Schreiben Sie JavaScript Code, um folgendes (ausschließlich) mittels dieser Art der Objekt-Orientierung zu realisieren: Alle Shape2D haben eine x und eine y Koordinate und unterstützen die parameterlose Methode mirror(). Der Rumpf der Methode kann leer gelassen werden. Rectangle erbt von Shape2D und bietet zusätzlich die parameterlose Methode area(). Auch hier kann der Methodenrumpf leergelassen werden. Square erbt von rectangle und überschreibt die Methode area(). my_square1 ist ein Square, bei dem sie dann x auf den Wert 10 und y auf den Wert 20 setzen (über direkte zuweisungen an die Attribute).
	- Jede JS Funktion hat automatisch ein Attribut prototype, der bei der Erzeugen der Funktion-Objekts mit einer leeren Objekt initializiert wird. Beim Erstellen eines Funktion-Objekts wird in der automatischen erzeugten prototyp Attribut ein Attribut constructor angelegt, das immer das Funktion-Objekt referenziert. Mittels new oder Object.create(Klasse) können neue Instanzen der Klasse erzeugt werden. new wird zusammen mit einer Funktion aufgerufen, welche dann die Rolle des Konstruktors zur Objekt-Initializierung übernimmt.
```javascript 
function Shape2D() {
	this.x = 0
	this.y = 0
}

Shape2D.prototype.mirror = function () {}

function Rectangle() {}
Rectangle.prototype = new Shape2D()
Rectangle.prototype.constructor = Rectangle
Rectangle.prototype.area = function() {}

function Square() {}
Square.prototype = new Rectangle()
Square.prototype.constructor = Square
Square.prototype.area = function() {}

let my_square1 = new Square()
my_square1.x = 10
my_square1.y = 20
```

Was versteht man unter prototypischer Vererbung (auch objekt-basierte Vererbung genannt) in JavaScript? Nennen und beschreiben Sie drei charakteristische Aspekte, die wesentlich in diesem Konzept sind. Dies können z.B. Aspekte sein, wie dieses Konzept „funktioniert“ oder z.B. zentrale Konstrukte der Programmiersprache, die in diesem Konzept relevant sind.
	- jede JS Function hat automatisch ein Attribut prototype, der zuerst mit einem leeren Objekt initializiert wird und dieser automatisch erzeugten Attribut hat ein Attribut constructor, der auf dem Function-Objekt zurückweist.
	- jedes Objekt kann ein anderes Objekt als Prototype haben, und mehrere Objekte können den gleichen Prototype haben, der dann zentrale Methode für alle diese Objekte implementiert
	- neue Instanze einer Klasse können mittels new oder Object.create erzeugt werden. new wird immer zusammen mit einer Funktions-Objekt aufgerufen, welches die Rolle des Konstruktors zur Objekt-Initializierung übernimmt

Gegeben sei folgender JavaScript Code. Zeichnen Sie das Diagramm der Objekte und ihrer Attribut-Beziehungen zueinander, dass sich bei der JavaScript-internen Umsetzung dieses Programmcodes ergibt. Orientieren Sie sich an der in der Vorlesung verwendeten Diagramm-Notation.
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

Erläutern Sie, was man bei JavaScript unter dem Begriff des "Polyfilling" versteht. (Den Code für das Polyfilling von Object.create() werde ich nicht erfragen. Auch nicht, wie man auf die Existenz testet und dann beim "fehlen" das Polyfilling macht. Aber als Beispiel für Polyfilling sollte man Object.create angeben können.)