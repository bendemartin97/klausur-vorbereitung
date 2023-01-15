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

##### Raid 1
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

##### Raid 5
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
- 