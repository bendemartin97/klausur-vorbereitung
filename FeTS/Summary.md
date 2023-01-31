### TODO
- Abläufe für RAID Vergrößern und Fehlerbeheben anschauen
- RAID Wie liest und schreibt ein RAID Array, wenn Platte defekt im Array ist?
- Nagios Commanden anschauen
### SMART
- Self-Monitoring, Analysis and Reporting Technology
- System zur Selbstüberwachung
- _Festplatten-interne Mechanismen protokollieren eine Reihe von Parametern_ #lernen
	- Realisiert im Festplatten Controller
	- in einem reservierten Bereich der Platte
	- kein Einfluss auf die Perfomance
- _für jeden Parameter protokolliert die Festplatte:_ #lernen
	1. Rohdaten: eigentlich gemessener Wert
	2. Übersetzter Wert: Skala bis 255 bis 0, aktueller und bisher schlechtester Wert
	3. Grenzwert: vom Hersteller festgelegt, Problem bei der Unterschreitung des Grenzwertes
- _Parameter-Typs:_ #lernen
	- pre-fail: warn vor baldigem Ausfall
	- old-age: zeigt Alterungszustand an
- _Self Test:_ #lernen 
	- prüft die Platte aktive elektrischen und mechanischen Eigenschaften und Lesedurchsatz
	- beim BS-Zugriff auf die Platte wird der Test unterbrochen
	- brechnen bei Fehlern an
- _Offline Test:_ #lernen
	- Ergänzung des Self Test, wird aber meist mit Self Test kombiniert
	- mehr Parameter werden erhoben
	- beim BS-Zugriftt wird unterbrochen
- _Architektur:_ #lernen 
	1. Festplatte und Festplatten-Controlle: 
		- Erhebung und Speicherung der "online" Parameterwerte, jeweils aktuellen Rohwert sowie "worst"
		- Ausführung der Offline Test, wenn getriggert, Erhebung und Speicherung der "offline" Parameterwerte
	2. Software auf den Rechner
		 - Abrufen der Parameterwerte
		 - Reporting der Werte
		 - Triggern der Offline-Tests
- _SMART mit Raid:_
	- direkte Zugriff aufgrund der Virtualisierung der Datenspeicher nicht möglich
	- SMART - Infos müssen über das Management des RAID ausglesen werden

### RAID #lernen
- Redundant Array of Inexpensive Disks
- _Ziel:_
	- Absicherung gegen Festplattenausfall
	- kein Datenverlust
	- keine Downtime im Betrieb
	- keine Auswirkungen des zusätlichen Sicherungsmechanismus
- _Idee:_
	- logische Laufwerke
	- intern aus mehreren physiche Laufwerken
	- Daten redundant speichern
- _Vorteile:_
	- höhere Verfügbarkeit der Daten bei Ausfall
	- bessere Perfomande (Datendurchsatz)
- _RAID Controller:_
	- hat die Rolle des Festplattencontrollers
	- stellt das RAID Array dem Host als eine logische Festplatte dar
	- sammelt Informationen über den Status der einzelnen Festplatten und stellt diese zur Verfügung
	- in Hardware
	- in Software, getrennt von der Implementierung des Dateisystems
	- in Software als Teil des Datensystems
### RAID Architektur
#### Host-Based RAID #lernen
- Teil des Host Systems
- Software RAID oder Hardware RAID
- einfache Lösungen belasten den Prozessor und die Datentransferbusse des Hosts stark
#### Software RAID #lernen
- keine Hardware für den RAID Controller 
- kann zu existierendem Rechner zur Laufzeut hinzugefügt werden
- Belaster CPU und Bussysteme des Rechners
- Arbeitsspeicher des Rechners als Cache
#### RAID Levels
- spezifiert die Mechanismen, nach denen das RAID System arbeitet
##### RAID 1 #lernen
- spiegelt die Daten über zwei oder mehr Festplatten des RAID 1 Arrays
- Redundanz der Daten: die gleichen Daten auf allen Platten, redundant bis zum n -2 Platten 
- Kapazität: Kapazität des kleinsten Platte
- sehr einfaches Funktionsprinzip
- festen Blockgröße von meist 64 kB
- hohe Ausfallsicherheit
- sehr einfaches Rebuild:
	- kopieren der Daten auf die neue Platte
	- geringe Komplexität
	- Datenverlust beim Ausfall aller Platten
- hohe Kosten für die Redundanz
- Hardweare RAID, ggf. Disk Array empfehlenswert
- erhöhte Leseperfomance durch Lesen von mehreren Festplatten
- Ausfall von bis zu n-1 Platten keine Auswirkung auf die operative Perfomance
- Schreiben der gespiegelten Nutzdaten erfordert zusätzliche Datentransfer-Kapazität zwischen Controller und Platte

##### RAID 5 #lernen 
- Daten werden über die Festplatten des RAID 5 Arrays verteilt
- Datenredundanz mittels Paritätsinformationen
- Paritätsblöcke gleichmäßig über alle Platten verteilt
- festen Blockgröße von meist 64 kB
- redundante Paritätsinformation erlaubt den Rebuild einer Platte bei deren Verlust
- erhöhte Leseperfomance durch Lesen von mehreren Festplatten
- Berechnung der Paritätsdaten erfordert Processing-Kapazität, und Schreiben der Paritätsdaten erfordert Datentransfer-Kapazität zwischen Controller und Platte
- Hardware RAID ist empfehlenswert
- geringe Kosten für die Redundanz
- Beim Ausfall einer Platte ist ein Rebuild erforderlich, um die Redundanz wiederherzustellen:
	- neue Platte erhält Nutz und Paritätsdaten 
	- Datenverlust bei einem weiteren Ausfall
- Schreiben einfache Variante:
	- Schreiben von Data1 auf die erste Festplatte.  
	 - Lesen von Data2, Data3, Data4 von den anderen Festplatten
	 - Berechnung der neuen Parität aus Data2, Data3, Data4 und neuem Data1
	 - Schreiben der neuen Parität auf die fünfte Festplatte.
- Schreiben verbesserte Variante:
	- Lesen des alten Werts Data1_old und der alten Parität Parity_old(Data1_old...Data4)
	- Schreiben von Data1 auf die erste Festplatte
	- Berechnung der neuen Parität aus Data1_old, Parity_old und neuem Data1
	- Schreiben der neuen Parität auf die fünfte Festplatte
- Hot Spare Disks sind schon in das RAID Array „verkabelt“, aber noch nicht aktiv eingebunden, d.h. keine Datenspeicherung

##### RAID 6 #lernen
- Erweiterung von RAID 5, Ausfall von bis zu zwei Platten kompensiert werden kann
- zwei Sätze von Redundanzinformation 
- Erhöhte Leseperformance durch Lesen von mehreren Festplatten des RAID 6 Arrays
- Datenintegrität des RAID 6 Arrays beim Ausfall von bis zu zwei Platten
- Rebuild:
	- Neue Platten erhalten Nutz- und Kontrolldaten gemäß dem RAID 6 Schema
	- können nach dem Ausfall einer Platte auch Lesefehler während des Rebuilds ausgeglichen werden (auch nach einem zweiten Ausfall)
- Schreibperformance ist nicht optimal (müssen zwei Kontrollblöcke geschrieben werden)

##### RAID 0 #lernen
- Just of a Bunch of Disks:
	- Datenspeicherkonfiguration mit mehreren unabhängigen Festplatten
- festen Blockgröße von meist 64 kB
- keine Redundanz
- Ausfall einer Platte ist schlimmer als bei n unabhängigen Platten
- Erhöhte Lese- und Schreibperformance durch Benutzung mehrerer Festplatten des RAID 0 Arrays

##### kombinierten RAID Levels #lernen
- Setups mit kombinierten RAID Levels können die Nachteile der individuellen RAID Levels ausgleichen
- Realisiert als RAID Array, bei dem jede (logische) oder einzelne Festplatten selbst wiederum ein RAID Array ist/sind
- Äußeres“ RAID Array und „innere“ RAID Arrays arbeiten nach verschiedenen RAID Levels

#### Aspekte des operativen Handlings #lernen 
- _Hot Spare Disk:_
	- sind schon in das RAID Array verkabelt, aber noch nicht aktiv eingebunden
	- die Fehlerhafte Festplatte kann sehr schnell durch eine Hot Spare Platte ersetzt werden
		- keine Beschaffung, physikalische Installation
		- Einbidung mittels RAID Rekonfiguration
		- erst mit dem Rebuild werden Daten auf die Platte geschrieben
- _Spare Disk:_
	- aktive Festplatte wird durch eine Spare Disk ersetzt, unter Nutzung der noch operativen Festplatte
	- Das RAID System kann die Daten der bisher aktiven Disk auf die Ersatzdisk kopieren
	- Vermeidet zwischenzeitlichen Verlust der Redundanz sowie aufwändigen Rebuild

#### Error handling #lernen
##### Festplatten und Controller
- internes Schreiben auf einen physischen Sektor fehlschlägt: remapping des logischen Sektors auf anderen physischen Sektor (reallocated sector) und werdenerneut gelesen, ob das Schreiben erfolgreich war
- Lesen eines logischen Sektors durch lesen des zugehörigen physischen Sektors fehlschlägt, erneute mehrfache Leseversuche, sonst ggfs. langer Timeout
##### SW Raid
- RAID nutzt Sector Reallocation der HDD für die Error Correction
- fehlerhafte Block wird mit den korrekten Daten, die irgendwo anders gefunden wurden, überschrieben und wieder gelesen
##### Timeouts
- während der sector reallocation HDD unresponsive
- wenn zu lange unresponsive HDD führt zu Timeout in der RAID Logik
- RAID markiert die ganze HDD als failed, obwohl noch gut funktioniert
- führt u lange Rebuild mit einer Spare Disk
- geringere Redundanz des RAID in dieser Zeit

### Clustersysteme #lernen
- eine Anzahl von vernetzten Computern, die von außen in vielen Fällen als ein Computer gesehen werden können
- die einzelnen Cluster-Knoten sind untereinander über ein schnelles Netzwerk verbunden
- _Ziel:_
	- erhöhung der Rechnenkapazität oder Erhöhung der Verfügbarkeit gegenüber einem einzelnen Computer
- _Hochverfügbarkeitscluster:_
	- Steigerung der Verfügbarkeit
	- beim Fehlerfall auf einem Knoten werden die Dienste und Ressources dieses Clusters auf andere Clusterknoten migriert
- _Clusterknoten:_
	- eigenständige Computer mit eigenem BS und Applikationen
- _Cluster Messaging:_
	- schnelle Kommunikation zwischen den Clusterknoten
	- Austausch von Lebenszeichen (heartbeat)
	- wichtig zur schennel Behandlung von Konfigurationsänderungen und Failures
- _Heartbeat:_
	- gegenseitige Benachrichtigung der Clusterknoten, dass sie betriebsbereit sind
	- regelmäßig gesendet
	- unicast oder multicast
	- typische Intervalle 1-5s
	- Ausbleiben mehreren aufeinanderfolgenden heartbeats löst Fehlerbehandlungsmechanismen in anderen Knoten aus
	- Gründe für das Ausbleiben:
		- verlorengegangene Nachrichten
		- Exzessive Verzögerung
		- Ausfall des Netzwerkinterfaces
		- Ausfall des Clusterknotens
- _Cluster Interconnect:_
	- optionale Separierung des cluster-internen Netzwerk vom öffenlichen
	- verhindert störende externe Einflüsse auf cluster-interne Kommunikation
	- erlaubt verscheidene Redundanz-Setups für beide Netzwerke
- _Cluster Membership:_
	- relevante Situationen:
		- Ausfälle von Clusterknoten und Ressourcen
		- initaler Start des Clusters
		- Hinzufügen oder Entfernen von Knoten
		- jeder Zeit unbedingt ein genaues und korrektes Bild der Membership
- _Cluster Manager:_
	- Steuer-Software des Clusters
	- läuft auf jedem Knoten -> die Steuerung läuft auch im Fehlerfall weiter
- _Cluster Information Base:_
	- zentrale Cluster Datenbank
	- im Cluster gespeichert und verwaltet von dem Manager
	- enthält alle Informationen über die aktuelle Konfiguration des Clusters
	- auf jedem Knoten verfügbar
- _Cluster Resource Manager:_
	- verwaltet CIB und behandelte alle Änderungen von clusterrelevanen Konfigurationsdaten
	- reagiert auf Events (z.B Ausfälle von Diensten)
	- Teil des Cluster Managers
- _Designated Coordinator:_
	- eine einzigartige zugewiesene Rolle
	- hält die Master-CIB
	- entscheidet über Beitritss-Requests neuer Knoten
	- Cluster-Konfigurationsänderungen zuerst im DC, welcher die Änderunegen in alle anderen Knoten propagiert
	- Auswahl bei Clusterstart: der erste aktive Knoten
	- Auswahlregeln klar und einfach -> bei Ausfall muss neue DC schnell gewählt werden
- _Cluster Ressourcen:_
	- auf Clusterebene verwaltete Dienste, Objekte
	- beinhaltet nicht die lokale Ressourcen der Clusterknoten
- _Local Resource Manager:_
	- auf jedem Clusterknoten
	- verwaltet lokalen Ressourcen
	- Änderungswünsche von CRM werden an den LRM delegiert
	- Logik zur Ressourceverwaltung
	- Adapter für das konkrete Handling der verschiedenen Ressourcen
	- Verwaltet die Resource Adapters
- _Resource Agents:_
	- abstrahiert von den konkreten Details der Ressourcen
	- einheitliche abstrakte Sicht von Seiten des Clusters 
	- Adapter für das konkrete Handling der Ressourcen
- _Score:_
	- beim Start einer Ressource im Cluster wird berechnet
	- eine Kennzahl, welche die Eignung des Knotens zur Aufnahme dieser Ressource festlegt
	- Plazierung der Ressource dann auf dem Knoten mit dem höchsten score
- _Resource Stickiness:_
	- Beharrungsvermögen einer Ressource auf einem Knoten
	- soll das zu häufige Wechseln einer Ressource zwischen verschiedenen Knoten erschweren
	- default Resource Stickiness gilt für alle Ressourcen auf allen Knoten
- _Constraints:_
	- Regeln, welche die scores der Clusterknoten bezüglich der Ressourcen beeinflussen
	- werden vom Cluster-Administrator definiert
	- Arten:
		1. Location: Platzierung von Ressourcen auf einem bestimmten Knoten
		2. Colocation: gemeinsame Platzierung von Ressourcen auf dem gleichen Knoten
		3. Ordering: Reihenfolge des Startens und Stoppens von Ressourcen
- _Resource Groups:_
	- Zusammenfassung von Ressourcen zu Gruppen
	- gemeinsame Administration
- _Migration von Ressourcen:_
	- das Monitoring der Ressourcen kann schon bei ihrer Definition spezifiert werden
	- übersteigt der fail-count einer Ressource ihren migration-threshold Parameter so wird die Ressource auf einen anderen Knoten migriert
- _Split Brain Situation:_
	- bei Störungen im Cluster-Interconnect entstehen mehrere Teilcluster
	- Jeder Teilcluster denkt, er wäre der einzig Überlebende oder der einzige korrekt funktionierende Teilsitzer
	- Jeder Teilcluster startet alle Ressourcen
	- konkurrierende Ressourcen, Inkonsistenzen, unkontrolliertes Verhalten
	- Maßnahmen:
		- Redundante Auslegung des Cluster Interconnet
		- Quorom Mechanismus: Auswahlmechanismus zur Bestimmung des einzig gültigen Teilclusters
- _Fencing:_
	- Ausschluss von einzelnen Ressourcen oder ganzen Knoten, deren Zustand unbestimmt ist
	- Resource-Level Fencing
	- Node-Level Fencing
	- Fehler im Cluster Interconnect, nicht stoppbare Ressourcen, ausbleibende Rückmeldungen, Software-Absturz

### Monitoring Systeme, Nagios
- _Ziele:_ #lernen
	- Ausfälle wenig Geld kosten
	- Planung von Durchführung von Infrastruktur-Upgrades
	- auf Anzeichnen von Störungen frühzeitig reagieren
	- in bestimmten Fällen automasiertes Lösen von Problemen
	- Koordination
	- Sicherstellung der Service Level Agreements
	- ganzheitliches Monitoring
#### Nagios
- überwacht Hosts, Services, Netztwerke
- erledigt durch Plug-Ins bestimmte Überwachungsaufgaben
- _Hosts in Nagios:_ #lernen
	- Geräte im Netztwerk, auf denen zu überwachende Services laufen
	- wird erst überwacht, wenn es mindestens zu einen zu überwachenden Service gibt
- _Host Resources:_ #lernen
	- spezifische Aspekte von Hosts, die überwacht werden können
	- Prozesstemperatur, RAM Speicherverbrauch etc.
	- werden als Services betrachtet
- _Host Checks:_ #lernen
	- in regelmäßigen Intervallen
	- nach Bedarf, wenn ein mit dem Host verbundener Service den Status wechselt
	- nach Bedarf als Teil der Host-Verfügbarkeits-Logik
	- nach Bedarf bei vorausschauenden Hosts-Abhängigkeitsprüfungen
- _Parent Hosts:_
	- Hierarchie der Hosts im Netzwerk, d.h direkte und indirekte Erreichbarkeit für Nagios
	- zum Entschieden ob ein Host wirklich nicht funktioniert, oder nur ein dazwischenliegender Parent Host nicht funktioniert
	- werden auch zwischenliegende Routers und Switches miteinbezogen
- _Host Groups:_ #lernen
	- mehrere Hosts zu gruppieren
	- Nagios Konfigurationen zu vereinfachen
	- werden im User Interface verwendet
- _Services:_ #lernen
	- SW-Dienst (HTTP, FTP etc.)
	- interne Eigenschaft eines Hosts (Speicher- und CPU-Auslastung)
	- messbare Umweltbedingung (Temperaturwert)
	- mit einem Host verbundene Information 
- _Service Checks:_
	- in regelmäßigen Intervallen
	- nach Bedarf bei voraussichtlichen Service-Abhängigkeitsprüfungen
- _Service Groups:_
	-  mehrere Services zu gruppieren
	- Nagios Konfigurationen zu vereinfachen
	- werden im User Interface verwendet
- _Statustypen : #lernen 
	- Soft Error:
		- Prüfungsergebnis in einem Nicht-Ok oder Nicht-Up Status
		- Prüfung noch nicht nach max_check_attempts oft durchgeführt wurde
		- werden nur die Eventhandler getriggert
	- Hard Error:
		- Prüfungsergebnis in einem Nicht-Ok oder Nicht-Up Status
		- Prüfung  nach max_check_attempts  oft durchgeführt wurde
		- von einem Hard-Error-Zustand in einen anderen Fehlerzustand wechselt
		- Service Prüfung mit einem Nicht-Ok-Status endet und der zugehörige Host down oder nicht erreichbar ist
		- trigger Eventhandler
		- Kontakte werden benachrichtigt
- _Flapping:_ #lernen 
	- durch öftere Zustandswechsels wird ein Sturm von Problemen- und Erholungsbenachrichtigung erzeugt
	- kann auf Konfigurationsproblemen, sich gegenseiteig störende Services, wirkliche Netztwerkprobleme oder anderweitige technische Probleme hinweisen
	- Ein Host oder Service wird eingestuft, mit dem Flapping begonnen zu haben, wenn der Prozentsatz das erste Mal den hohen Flapping-Schwellwert überschritten hat
	- Ein Host oder Service wird angesehen, das Flapping beendet zu haben, wenn der Prozentsatz unter den niedrigen Flapping-Schwellwert sinkt
	- Flapping Start:
		- Event-Meldung protokollieren
		- nicht permanenter Kommentar zum Host oder Service hinzufügen, dass er flattert
		- flapping-start Benachrichtigung an die betreffende Kontakte versenden
		- andere Benachrachtigungen unterdrücken
	- Flapping End:
		- Event-Meldung protokkolieren
		- den Kommentar löschen
		- flapping stop Benachrichtigung an die betreffende Kontakte versenden
		- die Blockade von Benachrichtigungen entfernen.

- _Publicy Available Services:_ #lernen 
	- von außen zugängliche Services eines Hosts
	- können von außen von Nagios geprüft werden
	- keine Add-On Software auf dem Host notwendig
	- HTTP, SMTP, ping, FTP, SSH
- _Aktive Checks:_ #lernen
	- gebräuchlichste Methode zur Überwachung
	- vom Nagios-Prozess veranlasst
	- laufen auf einer regelmäßig geplanten Basis
- _Private Services:_ #lernen 
	- nicht von außen zugängliche Services eines Hosts
	- Add-On Software (agent) auf dem Host erforderlich
	- RAM Speicherauslastung, CPU-Auslastung
- _Passive Checks:_ #lernen 
	- werden von Agent SW auf dem Host durchgeführt
- _Service Dependencies:_
	- Benachrichtungen und aktive Prüfungen von Services in Abhängigkeit vom Status eines oder mehrerer Services zu unterdrücken 
- _Plug-Ins:_
	- Programme oder Skripts, die von dern Kommendozeile aus den Status eines Hosts oder Services überprüfen
	- die Ergebnisse werden verarbeitet, um notwendige Aktionen auszuführen
	- arbeiten wie eine Abstraktionsschicht zwiscchen der Überwachungslogik im Nagion-Dämon und den eigentlichen Services, die überwacht werden
- _RRD:_
	- Round-robin-Database
	- ist ein Programm, mit dem zeitbezogene Messdaten gespeichert, zusammengefasst und visualisiert werden können
	- beim Anlagen der Datenbank wird genug Speicher für eine angegebene Zeitspanne angelegt
	- immer die ältesten Daten werden verdichtet
- _Business Process Add-Ons:_
	- überwachen in konsolidierten Form Geschäftsprozesse, die aus mehreren Hosts und Services bestehen
	- Dies ist insbesondere im Kontext von Service Level Agreements wichtig, da diese oft auf der Ebene der Auswirkungen auf die Geschäftsprozesse definiert sind.


### Elastic Stack
- Elasticsearch: Text und JSON basierende Suchmaschine. "Herz" des ELK Stacks
- _Logstash:_
	- sammelt, parsed und identifiziert Strukturen, transformiert und speichert Event-, Log- und anderen Daten-Input, z.B. Log-Dateien als datei-basierten Input, und sendet diesen transformierten und strukturierten Input an Elasticsearch oder andere Systeme.
- _Beats:_
	- ist die Plattform für Daten-Shipper, die jeweils genau für einen Anwendungsfall gebaut werden. Diese lassen sich als leichtgewichtige Agenten installieren und übermitteln Daten von Hunderten oder Tausenden Maschinen an Logstash oder Elasticsearch.
- _Kibana:_
	- für die browser-basierte, interaktive Analyse und Diagramm-Visualisierung der von Ergebnisdaten, insbesondere jener von Elasticsearch.  
	- Konfigurierbarkeit der Oberfläche: Dashboard.  
	- Viele verschiedene Diagrammtypen: Histogramme, Geo-Maps, ...
- _Volltextsuche:_
	- ~~Tokenizing: in Worte zerlegen
	- Stemming: Worte auf ihre Stammform reduzieren
	- Filtering: Füllworte weg
	- Ranking der Ergegnisse: wie viele Treffer im Dokument
	- Such-Index: schneller Suche
	- invertierter Index: Index der Suchbegriffe mit Verweisen auf die Vorkommen in den Dokument~~
	- persistente Datenspeicherung
	- REST API: Anfragen und Ergebnisse im JSON Format
	- Clustering: skalierende Perfomance, hohe Verfügbarkeit
	- ES ist schema-lose, dokumenten-orientierte NoSQL DB
	- Daten werden im JSON Format verarbeitet und gespeichert
	- Daten können einfach eingefügt werden
- _Index:_
	- Zusammenstellung von Dokumenten
	- gleich benannte Felder von Dokumenten im selben Index müssen vom selben Datentyp sein
- _Schemalosigkeit:_
	- beim ersten Erstellen eines Index durch Einlesen erster Daten ein Mapping anhand der Daten der bisheringen Dokumente automatisch aufgebaut wird
- _Scoring:_
	- Einstufung der Relevanz jedes Suchergebnisses 
	- Scorre Boosting beinflusst das Scoring basierend auf Regeln, die man angeben kann
	- beinflussende Faktoren:
		- Term Frequency: wie oft kommt der Term im Dokument vor?
		- Inverse Document Frequency: eie oft kommt der Term in allen Dokumenten des Index vor? Je häufiger, desto geringer die Gesamt-Relevanz
		- Field-Length Norm: je kürzer die Values des Treffer-Keys, desto relevanter
- _Cluster:_
	- bestehen aus mehreren ES Nodes
	- ES verhält sich wie ein gemeinsamer Dienst
	- Anfragen können an jeden Knoten des Clusters gerichtet werden
	- Anfragen werden automatisch verteilt
	- höhere Perfomance
### Event Management
- bezeichnet in der ITIL den Prozess zur Definizion von Filtern und Kategorisierung für Events
- Events häufig im Form von System-Alarmen und Logdatei-Einrägen
- _Prozess:_
	- Analyse von Trends und Mustern 
	- Pflege der Event-Monitoring-Regeln zur Filterung
	- Event-Filterung sollte automatisiert durchgeführt werden
	- Kontinuierliche Verbesserung des Event-Inputs

### Log Management
- Welche Arten von Infos aus Logs sind gewinnbar?
	- IT Sicherheit: Bedrohungen, Hack-Versuche
	- Troubleshooting: Systemfehler, Incidents
	- Systemperfomance
	- System Management: Welche Konfigurationsänderungen wo wann
	- Regulatorische Informationen: bestimmte Prozess- oder Sicherheitszertifierungen erfordern aktive Überwachund der Logs
- _Log Management Operationen:_
	- Log Collection & Archiving
	- Log Normalization
	- Log Monitoring: Log Correlation füge Zusatzinfos aus anderen Quellen hinzu
	- Benachrichtigungen, Alarmgenerierung
	- Reporting
- _Log Collection & Archiving:_
	- viele Logs aus vielen Quellen in vielen verschiedenen Formaten
- _Log normalization:_
	- binäre Logdaten in menschen-lesbares Format wandenl
	- extrahiere Datenfelder

### Incident Management
- _IT Service Management: _
	- Maßnahmen und Methoden, um Unterstützung von Geschäftsprozessen durch IT-Organisation
- _IT Infrastructure Library:_
	- Dokumentationsreihe
	- Sammlung bewährter, praxisbezogener Richtlinien und Verfahren
	- Planung, Bereitstellung, Betrieb und Management von IT Services
	- sind Prozesse, Rollen, Verantwortlichkeiten, Begriffsdefinitionen
- _Incident:_
	- ungeplante Beeinträchtigung bzw. Unterbrechung eines IT Services
	- Technisches Ausfall: System funktioniert gar nicht mehr
	- Perfomance incident: System funktioniert mit eingeschränkten Leistung
- _Incident management:_
	- der reaktive Prozess zur schnellst- möglichen, vollständigen Wiederherstellung des normalen Servicebetriebs
- _Support Organization:_
	-  verantwortlich für die Lösung der Incidents
	- oft strukturiert in drei Levels:
		1. Helpdesk:
			- first line to the customer
			- führt Erstklassifikation der Probleme durch
			- Löst möglich viele Standardprobleme sofort
			- Weiterleitung an 2nd Level
			- Kommuniziert Problemlösung und schließt das Ticket
			- Rückgriff auf CMDB -> schnelle Erschließung der spezifischen Situation beim Kunden
		2. 2nd Level Support:
			- weniger kompetentere Experte
			- in funktionalen Teams nach techn. Gebieten sub-strukturiert
			- Erstellen Standard-Lösungen für häufig wiederkehrende Probleme
		3. 3rd Party Product Support:
			- Werden Produkte anderer Hersteller in den eigenen Services oder Produkten verwendet, so muss ggf. die Support-Organisation des externen Herstellers in die Incident-Bearbeitung miteingebunden werden
- _Prozess:_
	- Erkennen des Incidents
	- Dokumentation
	- Direkte Lösung oder Weiterreichen
	- Lösung des Incidents
	- Dokumentation der Lösung
	- Nachprüfen der Lösung
	- Schließen des Incidents
- _Priorität:_
	- Bearbeitung der Incidents gemäß deren Priorität:
		- Reihenfolge der Bearbeitung
		- Umfang eingesetzter Ressourcen
	- Priorität = Auswirkungen * Dringlichkeit

### Ticket Systeme
- Software für Empfang, Bestätigung, Klassifizierung und Bearbeitung von Störungsmeldungen
- ermöglciht die Zuweisung eines Tickets an ein Team oder an eine Person zur weiteren Bearbeitung
- sicherstellen, dass keine Nachricht verloren geht
- _Issue Tracking:_
	- nicht nur Fehlern und Störungen
	- Strörungen
	- Anfragen nach Informationen, Preisen etc.
	- Änderungswünsche
- _Trouble Ticket:_
	- die elektronische Form einer Fehlermeldung im Fault Management
- _Queues:_
	- Mailbox für Tickets
	- gruppieren die Tickets inhaltlich
	- ein Ticket kann in eine andere Queue weitergeleitet werden
- _Eskalationen:_
	- Wird ein Ticket mit einer bestimmten Priorität vom aktuellen Support Level nicht innerhalb eines bestimmten Zeitraums gelöst, so wird es ggfs. vom Ticket System automatisch eskaliert
- _Vorteile:_
	- alle Tickets einheitlich dokumentiert und behandelt
	- automatische Meldung
	- online Überprüfung vom aktuellen Stand
	- Einheitlichkeit des Ablaufs
	- Analyse der Fallhistorien
	- eindeutige Kommunikation
	- Reporting

### Problem Management
- bezeichnet in der ITIL den proaktiven Prozess zur Ermittlung der eigentlichen Ursache für das (ggf.) wiederholte Auftreten von Incidents
- _Ziel:_
	- endgültige Beseitigungs der Störursache
	- oder Minimirung der Impacts zukünftiger solcher Incidents
- _Aktivitäten in Problem Management:_
	- Erkennen von Trends
	- Ursachenforschung
	- Erstellung von Lösungskonzepten
	- Initiierung von Request for Change
	- Reporting
