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
	- verbesserte Teamszusammenar ![[Bildschirm­foto 2023-01-15 um 11.31.08.png]]
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
