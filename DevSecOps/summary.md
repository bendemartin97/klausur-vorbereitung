### Softwareentwiclung
- Top 8 Arten der Softwareentwichlung:
	1. Wasserfallmodell  
	2. V-Modell  
	3. DevOps + DevSecOps mit agilem Entwicklungsmodell 
	4. Agile Softwareentwicklung– SCRUM,KANBAN  
	5. InkrementellesunditerativesModell,Prototypmodell 
	6. Extreme Programming  
	7. Spiralmodell, Rational Unified Process  
	8. Feature driven development, Leansoftware development
- effektivste Modell hängt stark ab vom :
	- zu entwickelten Projekt
	- Ressourcen and Budget
	- technischer Kenntnisstand, Erfahrung
	- Kunde
- keine allgemeingültige Antwort
### DevOps
- generell diverse Praktiken, Tools und eine (Unternehmens)-Philosophie
- _Ziele:_
	- mehr Softwarequalität
	- schnellere Softwareentwicklung
	- verbesserte Teamszusammenarbeit ![[Bildschirm­foto 2023-01-15 um 11.31.08.png]]
- _Pipelineaufbau:_
	1. Einrichtung eines CI-CD-Pipeline-Frameworks
	2. Integration mit einem Tool zur Quellcodeverwaltung
	3. Verbindung zum Buildautomatisierungstool
	4. Server-seitige Software-Bereitstellung
	5. Testen des Software-Codes
- _Vorteile:_
	- schnelleres Time-to-Market
	- häufigere Software-Releases
	- Überwachung und Transparenz
	- geringere Fehlerkosten
	- Automatisierung
- _Shift Left_
	- Bemühungen eines DevOps-Team die __Anweundungsicherheit in den frühesten Stadien des Entwicklungslebenzyklus__ zu gewährleisten
	
### Security by design
- IT-Sicherheit in Mittelpunkt steht
- _beeinflusst z.B:_
	- Design von Implementierungen
	- Desing von Softwareentwicklungsprozessen
	- Clean Code Policies
	- Qualitätsmaßnahmen
#### Bedrohungsmodellierung - Threat Modelling
- bei der Modellierung von Bedrohungen geht es um die Verwendung von Modellen zum Auffinden von Sicherheitsproblemen
- _Ziele:_
	- Auffinden und Verhindern von Sicherheitsproblemen
	- Auffinden von Problemen, die nicht bekannt sind
	- Produkte mit mehr Qualität entwickeln
	- Sicherheitsprobleme frühzeitig in Entwicklung angehen und beheben
- _Vetrauensgrenze:_
	- wer kontrolliert was? -> Trust boundaries
	- unterschiedliche Account-Level
	- unterschiedliche physikalische Rechner
	- virtuelle Maschinen
	- organisatorische Grenze
	- überall wo Unterschied zwischen Zugriffpriviligen gibt
- _Angriffsfläche:_
	- Teile des System, für die sich Angreifer interessieren
	- 3 Hauptbereiche:
		1. Netztwerk:
			- alle exponierten Netzwerkendpunkte, Geräte, Diesnte, BS, VMs, Container
		2. Anwendung:
			- Stellen, wo Daten oder Befehle eingegeben werden kann
			- insbesonders versteckte Endpunkte
			- Code hinter der Anwendung
		3. Mensch:
			- alle Personen, die an dem System arbeiten
- _4 Schritt Ansatz:_
	1. Was wird entwickelt?
		- das verändernde System modellieren
		- System verstehen
		- Systemdokumentation als Ausganspunkt
	2. Was könnte schief gehen?
		- anhand einer Bedrohunsmodell-Methodik Bedrohungen finden
		- Herzstück der Bedrohungsmodellierung
		- Kreativität eines pontenzielles Angreifer erforderlich
		- _Ansätze, um Bedrohungen zu finden:_
			- [[#STRIDE-Modell]]
			- Angriffsbäume
			- Angriffsbibliotheken
			- LINDDUN-Modell 
			- etc.
	1. Was könnte man dagegen tun?
		- die Bedrohungen priosieren
		- _4 Möglichkeiten:_
			1. Abschwächen
				- Maßnahmen, die die Ausnutzung einer Bedrohung erschweren
			2. Beseitigen
				- durch Beseitigung von Funktionalitäten / Funktionen
			3. Übertragen
				 - das Risiko von etwas anderem übernehem lassen
			4. Akzeptieren
				- Risiko akzeptieren
	1. Ist die Analyse korrekt?
		- durch ein IT-Sicherheitstestprogramm valadieren
		- sicherstellen, dass gefundene Security-Bedrogungen behoden wurden
		- für jede Bedrohung sollte ein guter Test existieren


### STRIDE-Modell
![[Bildschirm­foto 2023-01-15 um 14.05.07.png]]

### Schwachstellenmanagement
- alle Systeme, Anwendungen, Komponenten, Schnittstellen identifizieren, die pontenziell bedroht sind
- Verwendung von automatisierten Konfigurationsmanagement-Tool für Infrastrukturmanagement
### Schwachstellenscans
#### Standardablauf
1. Scannen der Server und Netzwerkanwendungen mit Tool wie Core Impact, Nessus
2. Überprüfung von gefundenen Schwachstellen, die filtern und priorisieren
3. Übergabe der Ergebnisse an das DevSecOps-Team
4. Erneutes Scannan, ob die Probleme beheben wurden
5. Dokumentieren

#### DevSecOps
- System muss öfter gescannt werden, wenn oft Änderungen vorgenommen werden
- Scannan automatisieren und Teil der Build-Pipeline werden lassen

#### Kritische Schwachstellen
- schnell handeln ist wichtig
- zeitnah viele Infos über Schwachstelle erhalten
- Fehler verstehen und Risiko anhand des CVE einschätzen
- alle Systeme identifizieren
- Feststellen, ob Systeme betroffen sind
- Plan für die schnelle Fehlerbehebung erstellen
- Prüfen, ob Maßnahmen erfolgreich waren

### Clean Code
- Sourcecode soll gut strukturiert, prägnant, leicht zu verstehen und lesen sein
- schlecht geschriebener Code erhöht das Risiko von Sicherheitsvorfällen
- bestimmte Code-Konstrukte sind aus IT-Sicherheitsaspekte besser 
### Validierung
- _2 Herausforderungen:_
	1. sicherstellen dass überall im Code alle Eingaben überprüft werden
	2. an allen Stellen definieren, was gültige Daten sind
- 5 Schritte:
	1. Ursprung:
		- prüfen woher die Daten kommen
		- Überprüfung der IP-Adresse
		- oder API-Schlüssels verwenden
	2. Größe
		- sinnvoll die Größe der Daten zu prüfen
	3. Lexikalischer Analyse
		- konzentriert sich auf den Inhalt und nicht die Struktur
		- Prüfung z.B ob erwartete Zeichen vorhanden sind
		- einfache Regexps sind gute Möglichkeit
		- bei komplexeren Eingabedaten Lexer-Tools verweden
	4. Syntax:
		- Prüfen ob Daten in richtigen Form vorliegen
		- kann wieder über Regexps oder über Code erfolgen
		- komplexe Datenstrukturen, wie XML, erfordern einen Parser
	5. Semantik
		 - Prüfen ob die Datek korrekt und konsistent sind und zum System passen
		 - Prüfen ob die Daten den Regeln und Einschränkungen des Domänenmodells entsprechen

### Sicheres Objekt-Design
- in OO Programmierung spielt eine große Rolle
- _wichtige Schutzziele:_
	- Datenintegrität: Konsistenz der Daten
	- Datenverfügbarkeit: Daten sind immer abrufbar
	- Datenvertraulichkeit: Schutz von Daten vor unbeabsichtigtem, unrechtmäßigem oder unbefugtem Zugriff

### Source-Code Reviews
- _Arten:_
	- formale Reviews 
	- Rubber Ducking
	- Pair Programming
	- Peer Code Reviews
	- External Source Code Audits
	- Automatisierte Code Reviews
- _Gründe für Source-Code Reviews:_
	- Fehler werden noch in der Entwicklungsphase erkannt
	- Code in den Coding-Richtlinien des Unternehmens festgelegten Standards entspricht
	- wiederholende Code Blöcke können erkannt werden
	- häufig verbreitete Schwachstellen können erkannt werden, wie Race-Conditions, Bufferüberläufe etc.
	- fungiert als Wissenaustausch