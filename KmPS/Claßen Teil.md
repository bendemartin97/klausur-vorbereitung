# Concurrency

### Concurrency
- bezeichnet die Möglichkeit, Programmteile unabhängig voneinander in unterschiedlicher Reihenfolge auszuführen zu können
- bei __mehreren__ Prozessoren oder Rechnern ist die parallele Ausführung möglich
- bei __nur einem__ Prozessor ist nur eine sequentialisierte Ausführung möglich, Reihenfolge variabel
- aber die Programmteile arbeiten gemeinsam an einer Aufgabe -> Abhängigkeiten
- _Abhängigkeiten werden gelöst in dem:_
	- mehrere Prozessoren/Rechner: Die Ausführung einiger Teile müssen warten oder ggf. pausiert werden
	- ein Prozessor/Rechner: Kontrollfluss muss wechseln, damit der Sender seine Aufgabe erledigen kann![[Bildschirm­foto 2023-01-13 um 19.41.00.png]]
	
### Parallelismus
- __bezeichnet die gleichzeitige Ausführung nebenläufiger Programmteile auf mehreren Prozessoren oder Rechnern__
- Falls während der Ausführung Abhängigkeiten über Daten oder Kommunikation auftauchen, die Ausführung einiger Teile müssen warten, ggf. pausiert werden![[Bildschirm­foto 2023-01-13 um 19.41.10.png]]

### Scheduler, Scheduling
- verteilt nebenläufige Programmteile / Tasks auf die zur Verfügung stehenden Ausführungseinheiten, wie z.B Prozessoren und kontrolliert bzw. bestimmt ggfs. ihre Ausführung
- _Der Scheduler:_
	- sichert den Prozesskontext des gerade unterbrochenen Tasks
	- wählt den nächsten Prozess aus
	- stellt dessen Prozesskontext her
	- gibt den Prozessor dann an diesen neuen Prozess ab
- Der Scheduler kann Listen mit verschieden priorisierten Tasks führen, und niedrig priorisierte entsprechend selten aufrufen.

### Kooperatives Scheduling (Phyton, JS, Go)
- __überlässt jedem Task selbst die Entscheidung, wann er die Kontrolle an den Scheduler zurückgibt__
- in der Regel wir zumindest jede Dienst-Anforderung an das Betriebssystem mit einem Taskwechsel verbunden
- _Vorteil:_
	- hilft Race Conditions zu vermeiden
- _Nachteil:_
	- kein schnelles Reagieren auf Ereignisse
	- Tasks, die die Kontrolle nicht schnell genug zurückgeben, können das gesamte System / andere Tasks (zeitweise) blockieren

### Mehrstufige Nebenläufigkeit in Go
- ein oder mehrere Prozessoren. 
- darüber mehrere Threads: Mindestens ein Thread pro Prozessor. Aber ggfs. auch mehrere Threads pro Prozessor. 
- darüber die Goroutinen.
- führt dazu, dass das Go Programm nicht unbedingt blockiert, wenn eine Goroutine die Kontrolle nicht zurückgibt: Wenn es noch andere Threads gibt, in denen nicht blockierte Goroutinen sind.

### Preemptive Scheduling (Java, Erland, Betriebssystemen)
- bezeichnet die Scheduling-Strategie, __bei welcher der Scheduler den Prozessen / Tasks eine Rechenzeit zuteilt und dem aktuellen Prozess die CPU nach Ablauf dieser Zeit wieder entzieht__, egal, an welcher Stelle der Prozess mit seinen Berechnungen gerade ist
- _Vorteil:_
	- der Flow of Control kann einem Task an beliebiger Stelle entzogen werden
	- schnelles Reagieren
- _Nachteil:_
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
- _Verwendung:_
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
- _ähnlich wie ein time.Sleep() , aber:_
	- Timer können vor Ablauf wieder gestoppt werden,
	- Timer-Setup ist nicht wie time.Sleep() synchron & blockierend, sondern per-se asynchron.

### Ticker
- werden gestartet und schicken dann wiederholt jeweils eine Nachricht mit dem Zeitstempelwert des jeweiligen Ticks auf einen ihnen jeweils zugeordneten Channel.
- __gedacht für wiederholt wiederkehrende zeitabhängige Aktionen.__

### Shared Data
- __in Go auch das Arbeiten mit Shared Data möglich.__ Also Prinzipien nicht so strikt wie z.B. beim Aktorenmodell von Erlang.
- _Mechanismen:  _
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
- __Jeder Aktor verfügt über einen Posteingang, eine Adresse und ein Verhalten__ ![[Bildschirm­foto 2023-01-14 um 10.39.32.png]]
- der Empfang einer Nachricht wird als __Ereignis__ bezeichnet -> werden __in Posteingang gespeichert__ und werden nach FIFO-Prinzip bearbeitet
- Das Verhalten des Aktoren beschreibt Reaktionen auf Nachrichten abhängig von deren Aufbau
- _Reaktionen:_
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
- _Unterscheidung mehrerer Server:_
	- der Server schickt seine Prozess-ID self() mit. Der Client macht Pattern Matching auf empfangene Nachrichten und matcht nur Nachrichten, die von der ID des selbst kontaktierten Servers stammen.
	- durch das Senden der ID:s in beide Richtungen können nun auch viele Server parallel laufen und sehr viele Clients parallel bedient werden, über das Netzwerk

### Erlang Prozesse
- __werden in der Erlang VM erzeugt und verwaltet__ -> OS unabhängig
- wenig Speicherverbrauch, sehr schnelle Erzeugung, schneller Prozesswechsel
- skaliert gut, auch bei parallelen Rechnern
- sehr fehlertolerant: wenn man alles auf viele kleine Einzelprozesse aufteilt, crasht bei Softwarefehler nur einer dieser Prozesse, nicht das Gesamte.
- __nur der Erzeuger eines Prozesses kennt diesen (dessen Prozess-ID) eigentlich__. Durch die __globale Registry__ können Clients die Server-ID:s herausfinden. Registrierung unter Aliasnamen. Konvention: Benutze den Modulnamen als Alias.

### Erlang Nodes
- ist eine Erlang Laufzeitumgebung
- __auf einem Host können mehrere gleichzeitig laufen__
- können aber auch __über mehrere mittels Netzwerk (auch Internet) verbundene Hosts__ verteilt werden
- wird über ihren Namen identifiziert -> müssen innerhalb eines Hosts eindeutig sein

### Erlang Cookies
- gruppieren __zusammengehörige Erlang Nodes__
- __Nodes können nur connecten, wenn Cookie übereinstimmt__
- gespeichert wird in Datei .erlang.cookie
- wird Cookie beim Start der Node nicht angegeben, wird er aus dieser Datei genommen
- Nodes haben auf gleichem Host immer identischen Cookie und können connecten

### Fehlerbehandlung in Erlang
- "Let it crash"
- _Best Practise:_
	- __Applikation in zwei Teilen aufbauen__: ein Teil, der die Aufgaben erledigt und einer der die Errors behebt
	- der __Teil, der das Problem löst, wird mit so wenig defensivem Code__ wie möglich geschrieben
	- der __Teil, der Fehler korrigiert, ist oft generisch,__ so dass derselbe fehlerkorrigierende Code für viele verschiedene Anwendungen verwendet werden kann.
	- dies führt zu einer __sauberen Trennung der Themen__ und zu einer drastischen __Verringerung des Codevolumens__.
- _bei parallelen Prozessen:_
	- Verknüpfung von Prozessen, die gemeinsam eine Aufgabe lösen mittels Link
	- __bidirektional__
	- wenn __eine der Prozesse crashed, sterben alle anderen linked Prozesse__ auch
- _System Prozesse:_
	- begrenzt die Fehlerpropagation
	- werden über Fehler benachrichtigt, sterben aber __nicht__
- _Monitoring von Prozessen durch andere Prozesse:_
	- unidirektional: Falls der ge-monitor-te Prozess stirbt, wird der Monitoring Prozess informiert, aber nicht umgekehrt.
	- Der Monitoring Prozess muss nicht System Process werden, um nur informiert zu werden, aber nicht selbst zu sterben.

### OTP
- stellt eine Reihe von Infrastruktur-Funktionalitäten zur Verfügung, um wirkliche "Praxistauglichkeit" in Bereichen wie Prozesshandling, Software Upgrade etc. zu erzielen.
#### Supervisor
- sind verantwortlich dafür, ihre __Kind-Prozesse zu starten, zu stoppen und zu monitoren__
- __überwachen Worker und/oder auch andere Supervisors__. Dadurch ergibt sich eine baumartige Struktur
- die Supervision Strategy gibt an, wie an den bestimmten Stellen im Baum bezüglich der Restart-Strategie zu verfahren ist, wenn ein Kind-Prozess stirbt
#### Worker
- sind für die normalen Berechnungen zuständig, ohne aufwendige Fehlerbehandlung
#### Restart Strategien
##### one_for_one
- bedeutet, dass, wenn Supervisor viele Workers beaufsichtigt und einer von ihnen ausfällt, nur dieser __eine neu gestartet__ werden sollte
- bei unabhängigen Prozessen
- Prozess kann seinen Zustand verlieren, ohne dass die Geschwisterprozesse davon betroffen wären![[Bildschirm­foto 2023-01-14 um 12.40.02.png]]
##### one_for_all
- ist immer dann zu verwenden, wenn __alle Ihre Prozesse unter einem einzigen Supervisor stark voneinander abhängen__, um normal funktionieren zu können![[Bildschirm­foto 2023-01-14 um 12.40.10.png]]
##### rest_for_one
- wenn ein Prozess stirbt, __alle nach ihm gestarteten Prozesse (die von ihm abhängen) neu gestartet werden__, aber nicht umgekehrt.![[Bildschirm­foto 2023-01-14 um 12.40.19.png]]
##### simple_one_for_one
- ein simple_one_for_one Supervisor sitzt einfach nur da und weiß, dass er nur eine Art von Child produzieren kann. 
- wann immer man __ein neues Kind haben möchte, fragt man danach und bekommt es. __
- theoretisch könnte man so etwas auch mit dem standardmäßigen one_for_one supervisor machen, aber es gibt praktische Vorteile, die einfache Version zu verwenden.
#### Software Patching im laufenden Betrieb
- [ ] in Vorlesung nachscheuen

# JavaScript
### Event Driven / Reactive Programming
- __Laufzeitumgebung des Programms triggert Events__
- __im Programm Callback registriert, der beim entsprechenden Event aufgerufen wird__
- JS Runtime immer __single-threaded__: Ein Thread mit einer Event Loop:
	- vermeide komplexe Anforderungen an die Plattform
	- JS Runtime ist ein großer Sichtbarkeitbereich -> Seiteneffekte
	- Parallele Event Loop könnten wegen Seiteneffekten die Semantik der Programme ändern
	- so wird jede Nachricht vollständig abgearbeitet
- _ScriptJobs Queue:_
	- vollständige Ausführung eines JS Skripts oder Moduls
	- wenn Jobs in Queue ist: führe den ersten Job aus
	- dann zurück zur Event Loop
	- __Skript / Modul muss abgearbeitet werden__, bevor der Browser wieder rendern kann
- _Nachteile_:
	- während eine Nachricht kann keine andere Nachricht abgearbeitet werden
	- langsame I/O Operationen blockieren die Ausführung

### Callback Programmierstil
- __asynchroner Code mittels Callbacks und synchrone Funktionsaufrufe__ dürfen bei Abhängigkeit voneinander (z.B. über Daten) nicht gemischt werden
- sonst werden die "__eigentlich sequentiell später kommenden Programmteile" ggfs. fehlerhafterweise vor den eigentlich "sequentiell vorher kommenden",__ aber asynchron (per Callback) programmierten Programmteilen ausgeführt.
##### falsch
```javascript
const fs = require('fs');
function readData() {

	let data1;
	fs.readFile( '/file.md', 
	(err, data) => { if (err) throw err; data1 = data; } );
	return data1;
}

// Das folgende console.log() des Ergebnisses wird ggfs. schon ausgeführt, obwohl // readData() mit seinem asynchronen fs.readFile() ggfs. noch gar kein Ergebnis geliefert hat! ...

console.log('User Name:', readData()); 
```


- Ebenso dürfen asynchrone Codeteile (mittels Callbacks), die voneinander (z.B. über Daten) abhängen, nicht einfach nur "sequentiell programmiert" werden. Sonst ist die Ausführungsreihenfolge unklar. __Verschachtelung der Callback Aufrufe notwendig.__
##### falsch
```javascript 
function asyncLogToConsole(data1) { /* ... execute async logging to console ... */ }
const fs = require('fs');
function readData() {

	let data1;
	fs.readFile( '/file.md', 
		(err, data) => { if (err) throw err; data1 = data; } );
	return data1;

}

var data = readData();

// Das folgende asyncLogToConsole() wird ggfs. schon ausgeführt, obwohl readData() // mit seinem asynchronen fs.readFile() ggfs. noch gar kein Ergebnis geliefert hat! ... 

asyncLogToConsole(data);
```
#### richtig
```javascript 
function asyncLogToConsole(data1) { /* ... async log to console ... */ }
const fs = require('fs');
function readData() {
	fs.readFile( '/file.md',
	          (err, data) => { if (err) throw err;
								asyncLogToConsole(data);
							}
);
}
````

### Callback Hell
- Die Programmierung der Nebenläufigkeit asynchroner Programmteile mittels Callbacks führt bei nacheinander auszuführenden (d.h. abhängigen) Programmteilen zu tief verschachteltem, unübersichtlichem Callback-Programmcode. ![[Bildschirm­foto 2023-01-14 um 15.34.07.png]]

### Reactive Programming
- higher order event-driven programmin
- ganze event streams als elementare Datenstrukturen ![[Bildschirm­foto 2023-01-14 um 15.38.06.png]]
### Nebenläufigkeit im imperativen Kontrollfluss
- ermöglichen die sequentielle Programmierung voneinander abhängiger asynchroner Programmteile
### Promise
- bezeichnet ein __Platzhalter-Objekt für ein Berechnungsergebnis, wobei deise Berechnung ggfs. noch nicht beendet ist__
- dieser Platzhalter kann vom folgenden, vom Wert anbhängigen Programmteil __direkt entgegengenommen werden__, ohne dass die vorherige Berechnung schon beendet sein muss
- wenn asynchrone Operation beendet ist, können von der Promise abhängige Programmteile zur Ausführung kommen
-  _Idee der Programmierung:_
	- Promises als Argumente an andere Prozeduraufrufe weitergereicht werden können.
- Zustand pending, wenn Ergebniswert noch nicht fertig berechnet.
- _Ansonsten settled:_
	- Entweder resolved  (Erfolgreich)
	- oder rejected (Fehler)
#### Executor Function
- ist __eine Funktion mit im Promise Standard festgelegter Signatur__ und stellt den Parameter des Promise Konstruktors dar. Sie wird sofort ausgeführt
- zwei Parameter: 
	- sind Funktionen, 
	- einen Wert als Parameter nehmen und diesen als "positives" bzw
	- Fehler-Ergebnis in das Promise Objekt eintragen.
	- "abstrakte" Parameter, vom JS System definiert.
- Nur der Rumpf des Executors wird vom Programmierer programmiert, inkl. der dortigen Aufrufe von resolve() bzw. reject()

```javascript 
function async_fun(...) {

	// ...  
	return new Promise( ( resolve , reject ) => {
			// Operationen, ggfs. asynchroner Art, 
			// zur "Berechnung" des konkreten Ergebniswerts 
				if ("async. Berechnung erfolgreich") 
					resolve(...) ; 
				else 
					reject(...) ;
	
	} );}
var promise1 = async_fun(...)
```

- __resolve() und reject () sind Callback-Funktionen__
- Warum gibt es diese Parametern überhaupt:
	- das Ergebnis nicht über Seiteneffekte "erzielen"
	- sondern als "pure function" in seinem Ergebnis nur von den Werten seiner Parameter abhängen
	- Werte in den Executor eingeschlossen -> können nicht mehr von der Außenwelt geändert werden
	- ohne denen Problemen bei der Fehlerbehandlung
	
```javascript
function resolves_after_2_s() { 
	let promise = new Promise( (resolve, reject) => {
		// Executor wird sofort ausgeführt ...
		// Nach 5 Sekunden (asynchron) wird das Promise 
		// Objekt auf den Wert 42 resolved

		setTimeout(
			callback: () => {
				resolve(42); console.log("Resolved.");
			}, 
			timeout: 5000); 
		});
		return promise;
}

let promise2 = resolves_after_2_s();
console.log("Script done.");

// Output:
// Script done.
// Resolved.
```

- resolve und reject ändern den Status des Promises auf settled -> __weitere resolve und reject werden ignoriert__
- _.then, .catch, .finally functions:_
	- then für daten-abhängigen Nachfolgecode
	- catch für Fehler-Callback
	- Callback Function als Parameter
	- Promise wird in den konreket Wert resolved dann wird Callback im .then ausgeführt
	- ._then() wird immer verzögert ausgeführt, auch wenn das Promise Objekt schon den Wert hat:_
		- zuerst ScriptJobs abarbeiten -> hängen then / catch Jobs an die PromiseJobs Queue an
		- PromiseQueue abarbeiten
		- Zurück zur EventLoop -> nächster ScriptJob abarbeiten
		- .then Callbacks werdem immer in der PromiseJobs Queue gescheduled
		- .then gibt immer ein Promis Objekt zurück -> können verkettet werden
	- _.catch für einheitliches Error handling_:
		- egal ob durch reject oder Exception im .then Rumpf
	- _.finally für cleanup Aktionen_:
		- hat kein Parameter
		- gibt den erhaltenen Resultat / Error Wert zurück
		- weiter Post-Cleanup-Aktionen möglich
- _Promise.all_:
	- warten auf das Settlement mehrerer Promises
	- bei asynchroner Plattform-Operationen werden parallel ausgeführt
	- Perfomance-Verbesserung
	- wenn ein Promise rejected, werden alle Ergebnisse verworden
- _Promise.allSettled()_:
		- sammelt die Ergebnisse und Fehler in Array
		- bei ein rejected Promise werden die Ergebnisse nicht verworfen
- _Promise.race_:
	- überwachen das Settlement mehrerer Promises
	- sammelt das Ergebnis der zuerst fertig werdenden Promise ein
	- die andere Egebnisse werden verworfen
### Promisification
- __Umwandlung einer Library mit API Funktionen im Callback-style in eine Variante der Library in Promise-style__
- Bei einer systematischen API der Library kann auch die Promisification der Library sehr systematisch gemacht werden, mittels einer generellen __Wrapper Funktion promisify(...)__

```javascript 
let function_promisified = promisify(orig_cb_function);

// Use the new, promisified function: ...

function_promisified(...).then(...);
```

- _Callback-style:_
	- wenn asynchronen Operationen merhfach verwendet werden, und die Callbacks bei jedem Aufruf andere Wert liefern
- _Promise-style:_
	- wenn asynchronen Operationen nur einmal vorkommen, an einer Stelle im Kontrollfluss
	- kann nur ein Resultatwert haben

### async / await
- asynchrone Funktionen
- im Rumpf kann mittels await auf die Resultate anderer Funktionen gewartet werden
#### async
- async gibt immer ein Promise Objekt zurück
- async ist also new Promise(...) 
- ihr Rump ist das Executor der neuen Promise
- await wartet auf das Settlement einer Promise, d.h die Zeilen hinter einem await sind der Parameter-Funktion eines .then()
- der return Wert der Funktion wird zum resolve() Wert der Promise

```javascript 
async function fun_async()  
{
	return 42;  
}

// Test: ...
fun_async().then(alert); // => 42
```

#### await
- ermöglicht, den daten-abhängigen Programmteil "sequentiell hinter dem await" zu programmieren statt im Callback-Stil als Parameter des .then()
- gibt den Kontrollfluss ab und wird getriggert, wenn die Promise des await in ihren Konkreten Wert resolved
#### Fehlerbehandlung
- mittels try / catch 
#### Problematiken des async / await
- await nur in async Funktionen oder auf dem top-level von Modulen
- viele await oder await in Schleifen verlangsamen den Code
- await ist asynchron -> in await-Pause können sich globale Variablen geändert haben, da Kontrolle abgegeben wurde

### Promise vs. async / await Style
- _Promise-style Notation:_
	- Die Promise-style Notation ist die "daten-orientierte Notation", denn Promises sind Datenobjekte und .then() etc. sind Methoden dieser Datenobjekte.
	- Daher auch keine "Notationsmarkierung" durch einen kontrollfluss-orientierten Befehl wie await, das passt da einfach nicht ...
	- Und Anwendungs-Logik in Funktionsparametern zu haben ist dann auch o.k., denn Funktionsparameter sind auch Datenwerte (vom Funktionstyp) und da passt das auch ganz prima ...
- _async / await-style Notation_:
	- async / await-style Notation ist "kontrollfluss-orientierte Notation". Kontrollfluss-orientierte Befehle (Befehle sind per-se kontrollfluss-orientiert...) async/await, abhängige Anwendungslogik ganz normal "im Kontrollfluss" notiert, Boilerplate Code zur Erzeugung des Promise-Datenobjekts "unterdrückt" (nicht explizit sichtbar).
# Closures
### Lexical Scoping
- bestimmt in vielen anderen funktionalen, imperativen und objekt-orientierten Sprachen die Sichtbarkeit und Gültigkeit von Bezeichnern (Variablennamen, Funktionsnamen) im Programm.
- bei __verschachtelten Funktionen sind die Bezeichner der äußeren Funktion auch in der inneren Funktion sichtbar__.
- gilt auch für globale Variablen
### Closure
- bedeutet, dass eine innere Funktion, die als Wert „im Programm umhergereicht“ wird, __den Zugriff auf die bei ihrer Erstellung von ihr benutzten Werte äußerer Funktionen behält__
- betrifft alle die Variablen in der inneren Funktion, die nicht als Funktionsparameter an die innere Funktion übergeben worden sind, sondern über das Lexical Scoping aus äußeren Funktionen in die innere Funktion gelangt sind

# Mutability & Immutable Data
### Mutability
- Veränderlichkeit, Wandlungsfähigkeit
- __Wertänderung eines Datenwerts, wobei die Objektidentität alle Referenzen auf das Objekt unverändert bleiben__
- z.B Arrays, Listen
- Problematisch bezüglich:
	- Programmkorrektheit
	- Parallelisierung von Code-Teilen
	- Sicherheitsaspekten
	- Abhängigkeiten in der Speicherung von Daten, die durch Daten-Mutation unterlaufen werden
### Immutable Date
- __darf nicht geändert werden, sobald der Wert initial erzeugt wurde__
- bei Konstanten: Wert zu Compile-Zeit berechnenbar
- Immutable-Data: initale Wert erst zur Laufzeit
- initiale Wert ist read-only -> darf nicht geändert werden, sobald einmal erzeugt ist
- ändernde Operationen sind nicht erlaubt, oder nur mithilfe einer Kopie
- sind änderbar, aber nicht auf dem gleichem Datum
- bei Änderungen verhält sich, alswürde  jede Objektreferenz immer auf ein seperates Objekt 
### Single-Assignment 
- erlauben nur einmalige Bindung einer Variable
- Rust, Erland: Redefinition von Variablen nicht erlaubt
- Elixir, Scala: Redefinition erlaubt
	- neue Variable hat mit der alten nichts zu tun
	- shadowing: alles, was sich auf die bisherige Variable bezog, nutzt auch weiterhin die bisherige

# Rust Mutability
- default immutable
- mittels mut -> mutable
### Data Copy in Rust
- wird vermieden -> Perfomance, keine Garbage Collection
- selbst definierte Datentypen default non-copy -> copy-operation erforderlich (Trait Copy)
- Rust Standard-Datentypen wie String (Heap-Speicher) non-copy
- Basis-Datentypen (int, float etc.) copy, da sehr effizient möglich
### Ownership
- bezeichnet das Konzept, dass immer eindeutig erkennbar (für den Compiler!) ist, wer der "Besitzer" eines Objekts ist und somit über die Freigabe von dessen Speicherplatz bestimmt
- sichere Speicherverwaltung ohne Garbage Collection
- Non-Copy Type zu einer anderen Variable wird die Ownership verschoben (auch als Wertparameter von Funktionen)
- ein Zugriff auf das Objekt über die alte Variable nicht möglich
- kein Move-Semantik bei primitiven Datentypen 
- benutzerdefinierten Datentypen Entscheidung mithilfe von Copy-Trait
### Pointer / Referenzen
- sind sichere Konstrukte -> Compiler kann sicherstellen, dass keine Speicherzugrifffehler über die Referenz geben wird
- _Sichere Eigenschaften_:
	- zeigen auf gültigen Speicher
	- dürfen nie null sein
	- keine Referenz existiert länger als das von ihr referenzierte Objekt
	- mutable Objekt wird nicht "anderweitig" verändert, während Mutable Referenz darauf existiert
	- eine mutable Referenz ist stets die einzige mutable Referenz auf ein Objekt
- _Non-Lexical Lifetime_
	- bedeutet, dass der Scope von Referenzen nur bis zu ihrer letzten Nutzung geht.

# JS Vererbung
### Prototype
- jede JS Funktion hat automatisch ein prototype Attribut
- beim Erzeugen eines Funktions-Objekts wird das prototype Attribut mit einem leeren Objekt initialisiert
- Beim Anlegen einer Funktion wird in dem automatisch angelegten prototype Objekt ein Attribut constructor angelegt, welches auf das Funktions-Objekt zurückverweist
### Prototypische Vererbung
- jedes Objekt kann ein anderes Objekt als Prototypen habe
- bei Lesen eines Objekt-Attributes wird erst im Objekt selbs gesucht, sonst wird der Prototypen-Verweiskette gefolgt
- mehrere Objekte können den gleichen Prototypen haben, der dann zentral Methoden für alle diese Objekte implementieren kann
- Schreiben, Hinzufügen etc. erfolgt immer im aktuellen Objekt, selbst es irgendwo in der Verweiskette schon existiert (wird ignoriert)
```javascript 
function Buch(titel){ this.titel = titel; } 
var ein_buch = new Buch("Der Buchtitel");
```

- _was der new Befehl leistet:_
	1. legt ein neues Objekt an
	2. ändert den Prototype Verweis des neuen Objekts auf der Konstruktor-Funktion im Prototype
	3. Konstruktor wird aufgerufen, bekommt das neue Objekt als Wert von this übergeben. Explizit angegebene Parameter des Konstruktors werden übergeben
	4. Wenn das Konstruktor einen Wert zurückgibt, wird es der Resultatwert der new Anweisung, sond das neu erzeugte Objekt
- Methoden müssen im prototype Objekt angelegt werden um für alle Objekt der Klasse gültig sein

```javascript 
function Buch (titel) { 
	this.titel = titel;
}

Buch.prototype.ablegen = function() { 
	console.log("Im Regal ablegen.");
}

var buch = new Buch("Mein Buch");
```

### OO Methodiken in JS
#### pseudo-klassische OO
- verwendet Klassen
- unterstützt durch Sprachkonstrukte (new)
- entspricht nicht dem Basis-Konzept
#### objekt-basierte OO
- realisiert die prototypische Vererbung
- selten genutzt
- jedes Objekt ist ein Singleton
- jedes Objekt hat eigene Methoden und kann auch Methoden eines anderen Objekts besitzen
- Verweiskette zwischen Objekten benutzt
- Erzeugung mittel Object.create
### Polyfilling
- vor ECMAScript 5
- Object.create Funktionalität mit selbst-definierten Funktion:
```javascript
function object_create(obj) { 
	function F() {}
	F.prototype = obj;
	return new F();
}
```

- das bisherige F Funktions-Object wird garbage-collected

# OO in GO
### Methoden
- sind Funktionen mit einem speziellen receiver Parameter