**Donnerstag 2. Februar 2023, 8:30-10:30 Uhr.** 
**Dauer der Klausur: 120 Minuten**

**Wichtig!:**
Diese Präsentation umfasst nicht alle Themen des Semesters aus meinem Teil. Somit gibt es auch nicht zu allen Teilen einen „Beispiel-Fragenkatalog“. Die anderen Themen sind aber genauso prüfungsrelevant: Alle meine Themen aus dem Semester sind komplett prüfungsrelevant! Die Idee der Prüfungsinfos ist ja, Ihnen konkret ein Verständnis zu geben, wonach ich in schriftlichen bzw. mündlichen KMPS Prüfungen frage, d.h. wie sie sich für die Prüfungen vorbereiten müssen / sollten. Dies wird beispielhaft anhand ausgewählter Themen des Semesters konkret gemacht. Dieselbe „Idee der Art zu fragen“ gilt dann auch für alle meine anderen Themen des Semesters!

## Fragen/Aufgaben

1. Beschreiben Sie, was man unter Concurrency und was man unter Parallelismus versteht. Grenzen Sie die beiden gegeneinander ab. 
	- Concurrency bedeutet, die Möglichkeit mehrere Programmteile unabhängig voneinander in verschiederner Reihenfolge auf einem oder mehreren Prozessor oder Rechner laufen zu lassen. Wenn mehrere Prozessoren verfügbar sind, ist es möglich die Programmteile parelell ausführen. Wenn nur ein Prozessor oder Rechner verfügbar ist, können die Programmteile nur sequentiell ausgeführt werden. Dabei arbeiten die Programmteile trotzdem zusammen, was zur Abhängigkeiten zwischen den Programmteile führen kann. Diese Abhängigkeiten werden gelöst, in dem bei mehreren Prozessoren, einige Programmteile warten ggf. pausiert werden müssen.
	- Parallelismus ist eine Art von Concurrency, wobei die Programmteile zur gleicher Zeit auf mehreren Prozessoren ausgeführt werden können.
2. Beschreiben Sie das Scheduling nebenläufiger Programmteile in Go und in Erlang und grenzen Sie diese beiden Scheduling Konzepte gegeneinander ab. 
3. Was versteht man unter kooperativem Scheduling? Nennen Sie dabei auch einen Vorteil und einen Nachteil dieses Scheduling Ansatzes. Beschreiben Sie das kooperative Scheduling in der Programmiersprache Go.
	- Unter kooperativem Scheduling versteht man, dass der Scheduler die Entscheidung selber dem Prozess überlässt, wann er die Kontrolle wieder zurückgibt. In der Regel sind jede Dienst-Anforderungen an das Betriebssystem mit einem Task-Wechsel verbunden
	- Ein Vorteil davon ist, dass das Konzept hilft Race Condition zu vermeiden. Ein Nachteil wäre, dass die Task die Kontrolle nicht schnell genug zurückgeben, das gesamte System blockieren können.
	- Bei der Programmiersprache Go wird auch das Konzept von kooperativem Scheduling umgesetzt, in dem Go Routine selber entscheiden können, wann sie die Kontrolle zurückgeben, wenn ein Go Routine idle oder eben blockiert oder explizit an besonderen Stellen im Programmfluss mittel runtime.GoSched
4. Wie wird der Wechsel der Kontrolle zwischen den nebenläufigen Goroutinen eines Go Programms veranlasst? Muss der Programmierer des Programms dies immer selbst programmieren?
	- In der Regel sind jede Dienst-Anforderungen an das BS mit einem Task-Wechsel verbunden. In Go wird die Kontrolle kooperative abgegeben, heißt wenn Go Routine idle oder blockiert oder an besonderen Stellen im Programmfluss explizit mittels runtime.GoSched. Wenn eine Go Routine zu lange die Kontrolle nicht zurückgibt, wird von dem Scheduler unterbrochen
5. Beschreiben Sie die Konzepte der Programmiersprachen Go und Erlang betreffend geteilte Daten zwischen nebenläufigen Programmteilen.
	- In Go heißt das Konzept Channels. Channels sind Kommunikationskanäle für Datenaustuch. Der Typ der zu transportierenden Nachricht muss schon bei der Erzeugung des Channels angegeben werden. Zwei Operationen sind Möglich: Senden und Empfangen. Wenn sich eine Go Routine auf einem Channel anschließt, wird direkt blockiert. Mittels dem select Befehl, kann es aber umgegangen werden. Es wichtig, dass jede Nachricht erfolgreich vermittelt werden können
	- In der Sprache Erlang wird das Konzept von dem Aktorenmodell umgesetzt. Dies ist ein Modell in Informatik für nebenläufige Berechnungen und Programme. Diese werden in nebenläufige Einheiten unterteilt, die ausschließlich über Nachrichtenaustauch kommunizieren. Jede Aktor verfügt über ein Posteingang, eine Adresse und ein Verhalten.
	- Der Empfang einer Nachricht wird als Ereignis bezeichnet und dies wird im Posteingang gespeichert. Das Verhalten des Aktores beschreibt die Reaktion auf eine Nachricht abhängig von deren Aufbau. Es gibt 3 verschiedenen Reaktionen: 1. die Nachricht an sich selbst oder an einem anderen Aktor zu senden 2. neue Aktoren zu erzeugen 3. eigenes Verhalten ändern. Das Aktoren Model legt nicht fest, wie lange die Vermittlung einer Nachricht dauern darf. Es ist nur definiert, dass jede Nachricht nach einer endlichen Zeit bei dem Empfänger an kommen muss. Der Nachrichtenaustausch erfolgt asynchron, d.h der Sender kann nach dem Absenden sofort mit einer anderen Aktion weitermachen und muss nich auf Bestätigung des Empfängers warten.
6. Nennen Sie den wesentlichen Unterschied zwischen Goroutinen und den Threads des zugrundeliegenden Betriebssystems, der dazu führt, dass Goroutinen nicht 1:1 auf Threads abgebildet werden.
	- Goroutine sind viel leichtgewichtiger als Threads. Ein Thread hat 1Mb Stack size, eine Goroutine nur 2Kb. Threads nutzen preemptives Scheduling, deswegen müssen viele Register gesichert werden, was zu einem teueren Context Switch führt. Goroutine verwenden hingegen kooperatives Scheduling in Go Runtime, was leichtgewichtigtere Context Switch ergibt. Goroutine werden vom dem Go Runtime verwaltet, Scheduling durch den sogenannten Green Threads. Dies passiert im user space von Go Runtime und nicht im kernel space. Dies führt dazu dass Goroutine auch auf OS:s ohne Thread Support funktionieren. 
7. Was versteht man in Go unter einem Tight Loop Szenario? Geben Sie ein Beispiel (Go Codeausschnitt) dafür an. Wie kann der Programmierer dieses Problem lösen?
	- Tight Loop beschreibt, dass eine Go Routine in einer sehr langen Schleife oder in einer Endloschleife läuft und dabei die Kontrolle nicht abgibt, heißt es werden keine Library Funktionen aufgerufen noch runtime.GoSched. Das heißt, in der Schleife werden nur elementare Operationen, also elementare Berechnungen durchgeführt. Der Programmieren kann Mechanismen in den Code abbauen, die die Schleife expizit abrechen (z.B time.Sleep(), oder mindesten die Kontrolle mittel runtime.GoSched abgibt.
		```GO 
var a = true
while(a) {
	const b = 2
}
	```
8. Beschreiben Sie, wofür Channels in Go genutzt werden. Welche Arten von Channels gibt es? Für welche Kommunikationsszenarien werden diese genutzt? Mittels welcher Operationen können die Goroutinen solche Channels nutzen? 
	- Channels sind Kommunikationskanäle, an denen sich Goroutine anschließen können. Mithilfe von Channels können Goroutine miteinander Nachrichten auszutauschen. Datentyp der zu übertragenden Werte müssen bei der Erzeugung angegeben werden. Es sind 2 verschiede Arten von Channels: Buffered und unbuffered Channels. Die ledigliche Unterscheid besteht zwichen den beiden dadrin, dass buffered Channels die Werte nicht nur übertragen, sondern auch speichern können. Es gibt 2 verschiedene Operationen bei Channels: Send und Receive. Für verschiedene Kommunikationsszenarien können Channels verwendet werden. Z.B für synchronisierten Beenden von main oder überall, wo wichtig ist mitzuteilen, dass z.B eine Goroutine mit seiner Berechnung durch ist.
9. Wie verhalten sich die grundlegenden send und receive Operationen bei Unbuffered Channels? 
	- Receive Operation: blockiert solange, bis eine Send Operation auf dem Channel ausgeführt wird, heißt ein Wert gesendet.
	- Send Operation: blockiert solange, bis eine Receive Operation empfangsbereit gemeldet hat. Sobald eine Receive Operation sich auf dem Channel gemeldet hat, wird die Send Operation fortgesetzt und der Wert überreicht.
10. Wie kann man Unbuffered Channels nutzen, was den Kontrollfluss von Goroutinen betrifft?
	- wesentliche Technik zur Synchronization
	- Unbuffered Channels können in Go verwendet werden, um den Kontrollfluss von Goroutinen zu steuern. Durch das Blockieren von Send- und Receive-Operationen können Sie sicherstellen, dass bestimmte Aktionen in der richtigen Reihenfolge ausgeführt werden und dass bestimmte Goroutinen auf bestimmte Ereignisse warten, bevor sie fortfahren.
11. Beschreiben Sie, welche Varianten des select Befehls es in Go gibt. Es ist nicht die exakte Syntax dieser Befehlsvarianten gefragt, sondern eine Beschreibung des Konzepts (der Semantik) der Varianten.
	- Recieve: 
		- select blockiert, bis auf einem der Channel eine Nachricht eintrifft
		- es ist auch mit timeout zu realisieren, um nicht zu lange zu blockieren
		- es ist auch mit default case zu realisieren, um auf einem Channel gar nicht zu blockieren
	- Send:
		- select blockiert, bis auf einem der Channel eine Nachricht versendet werden kann
		- es ist auch mit timeout zu realisieren, damit das select nicht zu lange blockiert
		- es ist auch mit default case zu realisieren, um zu vermeiden, dass select auf einem Channel blockiert wo noch keine Empfänger lauscht
12. Nennen Sie den wesentlichen Unterschied zwischen Erlang Prozessen und den Threads des Betriebssystems, der dazu führt, dass Erlang Prozesse nicht 1:1 auf Threads abgebildet werden.
	- Erlang Prozesse sind leicht-gewichtigte entkoppelte Erlang Prozesse
1. Nennen Sie den Namen des Nebenläufigkeits-Konzepts von Erlang und beschreiben Sie es
2. Begründen Sie, warum sich Erlang Prozesse über mehrere im Netzwerk verbundene Rechner verteilen lassen, ohne dass dies im Erlang Programm besonders berücksichtigt werden muss.
3. In Erlang möchte eine Funktion, die als Client agiert, einer anderen Funktion, die als Server agiert, einen Request (eine Anfrage, als Nachricht) schicken. Nennen Sie zwei Möglichkeiten, wie der Client den Server als "Nachrichtenziel" finden bzw. identifizieren kann, so dass die Nachricht dann beim richtigen Empfänger ankommt.
4. In einem Erlang Programm agiere eine nebenläufige Funktion als Client und eine andere nebenläufige Funktion als Server. Der Client schickt mehrere AnfrageNachrichten an den Server und erhält später mehrere Antwort-Nachrichten. Wie kann der Erlang Programmierer sicherstellen, dass der Client Code die empfangenen Nachrichten der richtigen entsprechenden (vorher geschickten) Anfrage zuordnen kann?
5. Beschreiben Sie die Best Practice Empfehlung, wie in Erlang Code mit Fehlersituationen umgegangen werden soll.
6. Bei der Erlang Programmierung gilt die Empfehlung: "Let it crash". Beschreiben Sie, wie im Erlang Code trotz crashender Prozesse die Fehlersituation eingegrenzt und strukturiert behandelt werden kann, so dass es nicht zu einem Crash des gesamten Applikationscodes kommen muss.
7. Was versteht man im Erlang OTP Framework unter einem Supervisor Prozess und einem Worker Prozess, wenn es um die Behandlung von Fehlersituationen im Erlang Code geht? Welche Rolle spielen die beiden im Kontext der in Erlang programmierten Applikation?
8. Wofür benutzt das Erlang OTP Framework die Restart Strategien? Beschreiben Sie zwei verschiedene dieser Strategien, inklusive Beschreibung eines von Ihnen erdachten Anwendungsfalls, in dem die jeweilige Strategie angemessen ist.
9. Bei Erlang Applikationen, die auf das Framework OTP aufgebaut sind, besteht die Möglichkeit, bei Teilen der Applikation zur Laufzeit Code auszutauschen. Beschreiben Sie, welche Eigenschaften Erlang als Sprache besitzt, auf die diese Möglichkeiten dann aufbauen können.
10. JavaScript Interpreter bieten im Prinzip nur eine "Ausführungseinheit" für den JavaScript Code und nur eine Event Loop. Trotzdem ist es möglich, nebenläufige Programmteile zu programmieren.
	1. Beschreiben Sie einen Mechanismus, durch den bestimmt wird, ob und wann die nebenläufigen Programmteile zur Ausführung kommen. 
	2. Zu welchen negativen Konsequenzen können die Einschränkungen "eine Ausführungseinheit" und "eine Event Loop" trotz des Vorhandenseins von Nebenläufigkeit führen, wenn man ausschließlich die Ausführung von JavaScript Code betrachtet? Denken Sie sich ein Beispiel aus, welches diesen Effekt illustriert, und beschreiben Sie es high-level.
11. Was versteht man unter dem JavaScript Callback Programmierstil? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das Callback Prinzip illustriert, und erläutern Sie ihr Beispiel.
12. Wie kann man bei der JavaScript Programmierung mittels einfacher Callbacks die Fehlerbehandlung programmieren? Geben Sie ein JavaScript Codebeispiel mit einer von ihnen erdachten JavaScript Callback Funktion an, welches illustriert, wie die Fehlerinformationen einer asynchronen Operation an ein Callback übergeben und von diesem verarbeitet werden könnten. Das Beispiel soll auch die Registrierung der Callback Funktion beinhalten.
13. Was versteht man bei der JavaScript Programmierung unter dem Problem der "Callback Hell" (oder "Pyramid of Doom")? Geben Sie ein JavaScript Codebeispiel mit von ihnen erdachten JavaScript Funktionen an, welches das "Callback Hell" Problem illustriert, und erläutern Sie ihr Beispiel.
14. Erläutern Sie anhand eines von Ihnen erdachten JavaScript Pseudo-Code-Beispiels das Konzept des "Reactive Programming". Erläutern Sie insbesondere, auf welcher Art von Datenstrukturen dieser Ansatz basiert und welche Art von Operationen auf diesen Daten benutzt werden. Beschreiben Sie, welchen Vorteil dieser Programmieransatz bietet gegenüber der klassischen event-basierten Programmierung in JavaScript.
15. Was versteht man bei der JavaScript Programmierung unter dem Konzept der Promises? Was ist eine Promise? Wie wird sie im JavaScript Code programmiert? Geben Sie ein von Ihnen erdachtes JavaScript Pseudo-Code-Beispiel an, in dem eine von Ihnen erdachte JavaScript Funktion eine Promise erzeugt und zurückgibt. Erläutern Sie den Zweck der Promise mittels ihres Beispiels. Erläutern Sie auch, aus welchen Codekonstrukten sich die Promise zusammensetzt und wie diese Konstrukte in ihrem Beispiel genutzt werden, damit die Promise ihren Zweck erfüllen kann.
16. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise die Funktion resolve() einmal mit dem Parameterwert 11 aufgerufen wird (resolve(11)) und anschließend mit dem Parameterwert 22 aufgerufen wird (resolve(22))? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt. Was passiert bei einer JavaScript Promise, wenn im Executor der Promise zuerst resolve(33) aufgerufen wird und anschließend reject(-1) (d.h. Fehlercode -1)? Erläutern Sie insbesondere, zu welchem Ergebniswert und Status der Promise dies führt.
17. Es ist im JavaScript Code auf keine der folgenden alternativen Arten möglich, die Beispiel-Promise zu resolven: 
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