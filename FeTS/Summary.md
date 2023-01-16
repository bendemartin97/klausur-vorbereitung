### Grundlagen
- _Verfügbarkeit:_
	- ein System ist verfügbar, wenn es in der Lage ist, die vorgesehenen Aufgaben zu erfüllen
	- Produktionszeit / (Produktionszeit + Ausfallzeit) * 100
	- Gesamtverfügbarkeit hängt ab von der Verfügbarkeit und der logischen Anordnung der Teilsysteme
	- Serielle Anordnung: Gesamtfunktionalität nur dann gegeben, wenn alle Teilsysteme funktionieren
	- Parallele Anordnung: bei gleicher Funktionalität (Redundanz), solange ein Teilsystem arbeitet, ist die Gesamtfunktionalität prinzipell noch gegeben 
- _Downtime:_
	- bezeichnet die Zeit, in der ein Computersystem nicht verfügbar bzw. nicht funktionstüchtig ist
	- scheduled downtime: Wartungsarbeiten, geplante Änderungen
	- unplanned downtime: Fehler in Systemkomponenten, Bedienung, Umgebung
	- Service Level Agreements definieren, ob scheduled downtime in die Berechnung der Verfügbarkeit eingeht oder nicht (default nicht)
- _Mean Time Between Failures_:
	- mittlere Betriebszeit zwischen zwei aufeinanderfolgenden Ausfällen
	- ist ein Maß für die Zuverlässugkeit des Systems
- _Mean Time To Failure_:
	- bei nicht reparierbaren Komponenten oder bei denen, die nach der Reparatur neuwerting sind
	- bezeichneit die mittlere Betriebsdauer bis zu Ausfall -> mittlere Lebensdauer
	- statische Kenngröße
- _Hochverfügbarkeit:_
	- Verfügbarkeit liegt oberhalb einer Minimalerwartung
	- 99,99 %, also 52:36 Minuten in hochverfügbaren IT-Systeme
	- 99,999 %, also 5:16 Minuten in Telekommunikationsnetzte
	- wird durch Fehlertoleranz gewährleistet
- _Fehlertoleranz:_
	- Anwendung auch im Fehlerfall weiterhin verfügbar
	- ohne unmittelbaren menschlichen Eingriff weiter nutztbar
	- Anwender nimmt keine oder nur kurze Unterbrechung wahr
- _Sinple Point of Failure:_
	- eine einzelne Komponente, deren Versagen zum Ausfall des gesamten Systems führ
	- Vermeidung durch:
		- Redundanz von Komponenten
		- Fehlertolerantes und robustes Systemverhalten
- _Redundanzkonfigurationen:_
	- Active / Active:
		- im Nicht-Fehlerfall alle Dienste auf allen Systemen aktiv
		- im Fehlerfall fallen einzelne Dienstinstanzen weg, alle anderen arbeiten
	- Active / Passive:
		- Passive Dienstinstanzen unaktiv
		- im Fehlerfall werden sie Ersatz für die fehlerhaften Dienstinstanzen
		- Hot Standby: Systemstart und Aufwand für das Übertragen der aktuellen Konfiguration etc. minimiert
		- Cold Standy: einfacher zu realisiren, aber längere downtime und höherer Aufwand zur Behandlung des Fehlerfalls
	- 1 + 1 Redundanz: 
		- Pro aktivem System ein passiver Standby
		- Verschwendung der Ressources des passiven Systems
	- N + 1 Redundanz:
		- bei mehreren aktiven System ein passiver Standby
	- M + N Redundanz:
		- bei mehreren aktiven System gibt mehrere passive Standbys
- _Out of Band Management:_
	- Administraton eines Systems über einen Zugriffspfad
	- getrennt vom Datenpfad deer Applikationsdaten des Systems
	- z.B seperates Netzwerk für die Systemadministration
	- verhindert, dass Netzwerkprobleme im Bereich der Applikationsdaten auch die Administration des Server-Systems behindern oder verhindern
- _In-Band Management:_
	- Administration eines Systems über den gleichen Netzwerkzugang

### Festplatten
- _Datenspeicherung auf Festplatten:_
	- Daten werden in Blöcken gelesen und geschrieben
	- Partitionen: Unterteilung der Festplatte in unabhängige Bereiche
	- Dateisystem: Strukturinformation für die Datenspeicherung
	- Festplattezugriffe langsamer als Zugriffe auf PRozessor Cache und RAM Speicher
	- Caching der Daten im RAM des Computers
	- beim Ausfall gelangen die Cache-Inhalte nicht mehr auf die Festplatte
	- Server-Festplatten haben eine höhere MTTF als Desktop-Festplatten
- _Ausfallursachen bei Festplatten:_
	- Fehler in Steuerelektronik
	- Verschleiß der Mechanik
	- Mechanisches Aufsetzen des Schreib-Lesekopfes aufgrund von Bewegungen
	- thermische Probelem
### SMART
- Self-Monitoring, Analysis and Reporting Technology
- System zur Selbstüberwachung
- Festplatten-interne Mechanismen protokollieren eine Reihe von Parametern
	- Realisiert im Festplatten Controller
	- in einem reservierten Bereich der Platte
	- kein Einfluss auf die Perfomance
- für jeden Parameter protokolliert die Festplatte:
	1. Rohdaten: eigentlich gemessener Wert
	2. Übersetzter Wert: Skala bis 255 bis 0, aktueller und bisher schlechtester Wert
	3. Grenzwert: vom Hersteller festgelegt, Problem bei der Unterschreitung des Grenzwertes
- Parameter-Typs:
	- pre-fail: warn vor baldigem Ausfall
	- old-age: zeigt Alterungszustand an
- Self Test:
	- prüft die Platte aktive elektrischen und mechanischen Eigenschaften und Lesedurchsatz
	- beim BS-Zugriff auf die Platte wird der Test unterbrochen
	- brechnen bei Fehlern an
- Offline Test:
	- Ergänzung des Self Test, wird aber meist mit Self Test kombiniert
	- mehr Parameter werden erhoben
	- beim BS-Zugriftt wird unterbrochen
- Architektur:
	1. Festplatte und Festplatten-Controlle:
		- Erhebung und Speicherung der "online" Parameterwerte, jeweils aktuellen Rohwert sowie "worst"
		- Ausführung der Offline Test, wenn getriggert, Erhebung und Speicherung der "offline" Parameterwerte
	2. Software auf den Rechner
		 - Abrufen der Parameterwerte
		 - Reporting der Werte
		 - Triggern der Offline-Tests
- SMART mit Raid:
	- direkte Zugriff aufgrund der Virtualisierung der Datenspeicher nicht möglich
	- SMART - Infos müssen über das Management des RAID ausglesen werden

### RAID
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
#### Host-Based RAID
- Teil des Host Systems
- Software RAID oder Hardware RAID
- einfache Lösungen belasten den Prozessor und die Datentransferbusse des Hosts start
#### RAID Levels
- spezifiert die Mechanismen, nach denen das RAID System arbeitet
##### RAID 1
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

##### RAID 5
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

##### RAID 6
- Erweiterung von RAID 5, Ausfall von bis zu zwei Platten kompensiert werden kann
- zwei Sätze von Redundanzinformation 
- Erhöhte Leseperformance durch Lesen von mehreren Festplatten des RAID 6 Arrays
- Datenintegrität des RAID 6 Arrays beim Ausfall von bis zu zwei Platten
- Rebuild:
	- Neue Platten erhalten Nutz- und Kontrolldaten gemäß dem RAID 6 Schema
	- können nach dem Ausfall einer Platte auch Lesefehler während des Rebuilds ausgeglichen werden (auch nach einem zweiten Ausfall)
- Schreibperformance ist nicht optimal (müssen zwei Kontrollblöcke geschrieben werden)

##### RAID 0
- Just of a Bunch of Disks:
	- Datenspeicherkonfiguration mit mehreren unabhängigen Festplatten
- festen Blockgröße von meist 64 kB
- keine Redundanz
- Ausfall einer Platte ist schlimmer als bei n unabhängigen Platten
- Erhöhte Lese- und Schreibperformance durch Benutzung mehrerer Festplatten des RAID 0 Arrays

##### RAID 2
- Daten werden bitweise über die Festplatten des RAID 2 Arrays verteilt
- Führt dazu, dass Anzahl der Datenplatten typischerweise der Anzahl der Bits in einem Datenwort entspricht, also typischerweise 32 Festplatten
- Pro Datenwort wird ein Hamming Error Correction Code berechnet, dessen Bits werden auf die zusätzlichen ECC Festplatten verteilt
- Redundante ECC Information:
	- schützt gegen den Ausfall einer Festplatte
	- schützt auch gegen Schreibfehler auf irgendeiner der Festplatten

##### RAID 3
- Daten werden über die Festplatten des RAID 3 Arrays verteilt
- Plus Datenredundanz mittels Paritätsinformation.
- Paritätsblöcke auf eigener Platte gespeichert
- Keine standardisierte Blockgröße
- Paritätsfestplatte wird zum Engpass bei Schreiboperationen
- Datenintegrität des RAID 3 Arrays beim Ausfall von maximal einer Platte
- Beim Ausfall einer Platte ist ein Rebuild erforderlich

##### RAID 4
- Striping und Paritätsinformation auf einer dedizierten Festplatte wie bei RAID 3
- festen Blockgröße von meist 64 kB
- Paritätsfestplatte wird zum Engpass bei Schreiboperationen
- Datenintegrität des RAID 4 Arrays beim Ausfall von maximal einer Platte

##### kombinierten RAID Levels
- Setups mit kombinierten RAID Levels können die Nachteile der individuellen RAID Levels ausgleichen
- Realisiert als RAID Array, bei dem jede (logische) oder einzelne Festplatten selbst wiederum ein RAID Array ist/sind
- Äußeres“ RAID Array und „innere“ RAID Arrays arbeiten nach verschiedenen RAID Levels

#### Datenintegrität
- Ziel ist Schutz vor dem Ausfall einer oder mehreren Festplatten
- Datenintegrität wird normaleweise nicht berücksichtigt (ausnahme RAID 2)
- einige RAID Controller haben Korrekturmechaniscmen:
	- read after write
	- Vegleich mit den von den Platte gelesenen Daten
	- ggfs. Korrektur der falschen Daten
	- Cache der Platte muss aus sein
- RAID Scrubbing bei RAID 5 / 6:
	- korriegiert bei gefunderen Inkonststenz in einem Stripe nur die Parität dieses Stripes
- Datenfehler auf Applikationsebede können nicht von RAID behandelt werden
- kein Ersatz für Datensicherung
#### Dateisysteme mit integriertem RAID
- sind Dateisystemfunktionalität und RAID Funktionalität integriert (ZFS, Linux btrfs)
- Vorteil:
	- RAID kennt belegte und freie Blöcke
	- behandelt bei Rebuild nur belegte Blöcke -> schneller
	- Dateisystem kann über Prüfsummen die Dateiintegrität überwachen
- Nachteil:
	- Mechanismen sind nicht separiert
	- erhöhte Komplexität und Fehleranfälligkeit
	- Hardware-Unterstptzung schwer zu realisieren
#### Kombination von Volume Management und RAID
- als Kombination seperater Funktionalitäten
- oder integriert in die LVM Funktionalität
##### LVM mit Integriertem RAID 1 ![[Bildschirm­foto 2023-01-16 um 09.17.50.png]]
##### Realisierung der PV:s als RAID Arrays
- geringe Integration und Interaktion von LVM und RAID
- modulare Realisierung
- klar abgegrenzte Funktionalität, Administration und Fehlerhandling
- gut für HW-Unterstützung der RAID Funktionalität
- ![[Bildschirm­foto 2023-01-16 um 09.21.27.png]]
##### RAID Funktionalitäten integriert in dem LVM
- hohe Integration
- entweder über API getrennt implementiert
- oder gemeinsame, integrierte Implementierung
- keine HW-Unterstützung der RAID Funktionalität
- komplexeres, manuelles Handling des Rebuilds 
- ![[Bildschirm­foto 2023-01-16 um 09.22.23.png]]

### Clustersysteme
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
- _Ziele:_
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
- _Hosts in Nagios:_
	- Geräte im Netztwerk, auf dendn zu überwachende Services laufen
	- wird erst überwacht, wenn es mindestens zu einen zu überwachenden Service gibt
- _Host Resources:_
	- spezifische Aspekte von Hosts, die überwacht werden können
	- Prozesstemperatur, RAM Speicherverbrauch etc.
	- werden als Services betrachtet
- _Host Checks:_
	- in regelmäßigen Intervallen
	- nach Bedarf, wenn ein mit dem Host verbundener Service den Status wechselt
	- nach Bedarf als Teil der Host-Verfügbarkeits-Logik
	- nach Bedarf bei vorausschauenden Hosts-Abhängigkeitsprüfungen
- _Host Groups:_
	- mehrere Hosts zu gruppieren
	- Nagios Konfigurationen zu vereinfachen
	- werden im User Interface verwendet
- _Services:_
	- SW-Dienst (HTTP, FTP etc.)
	- interne Eigenschaft eines Hosts (Speicher- und CPU-Auslastung)
	- messbare Umweltbedingung (Temperaturwert)
	- mit einem Host verbundene Information 