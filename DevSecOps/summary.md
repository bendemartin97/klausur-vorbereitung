[[#Kapitel 3]]
	    [[#Motivation Automatisiertes Testing]]
	    [[#Automatisierter Software Testing Life Cycle]]
	    [[#Use Cases für Automatisiertes Testing]]
	    [[#Automatisiertes Testen vs Manuelles Testen]]
	    [[#Bekannte Testautomatisierungsframeworks]]
	    [[#Security Testing]]
	    [[#SAST]]
	    [[#DAST - dynamic application security testing ]]
	    [[#Snyk]]
	    [[#Zusammenfassung K3]]
    [[#Kapitel 4]]
	    [[#CI/CD]]
	    [[#Application Deployment]]
	    [[#Cloud-Deploymentmodelle]]
	    [[#Microservice Architektur]]
	    [[#Serverless Architekturen]]
	    [[#Containerisierung]]
	    [[#Container-Architekturen ]]
	    [[#Best Practices]]
	    [[#Zusammenfassung K4]]
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

### Kapitel 3 - Security Testing & SAST
[PDF Link](DevSecOps_Kapitel3.pdf)
-   __Hauptziele des Security Testing__
    -   Testen ist eine wichtige Tätigkeit im _SDLC_, insbesondere bevor der Kunde die Software erhält
    -   Security Testing legt einen Fokus auf Fehler, die als Schwachstellen ausgenutzt werden können
    - _Hier Fokus auf automatisierte Tests_
        
-   __Hauptziele des Static Application Security Testing__
    -   Fehler im Source Code und potentielle _Schwachstellen frühzeitig finden (während der Entwicklung)_
    -   _Automatisierte Tools unterstützen die Entwickler_ und machen eine sichere Entwicklung erst möglich


#### Motivation Automatisiertes Testing
[PDF Link](DevSecOps_Kapitel3.pdf#page=5)
- Kostenreduktion
- Erhöhte Softwarequalität
- Mehr Jundenzufriedenheit
- Wiederverwendbar
- Programmierbar

#### Automatisierter Software Testing Life Cycle
[PDF Link](DevSecOps_Kapitel3.pdf#page=6)
![[Pasted image 20230116091908.png|500]]

#### Use Cases für Automatisiertes Testing
[PDF Link](DevSecOps_Kapitel3.pdf#page=7)
##### Regression Testing
	eignen sich für automatisierte Tests, da der Code häufig
	geändert wird und die menschliche Fähigkeit zur rechtzeitigen
	und qualitativen Durchführung von Tests überfordert ist.
##### Load testing
	Es soll überprüft werden, ob das System/die Anwendung die
	erwartete Anzahl von Transaktionen bewältigen kann, und das
	Verhalten des Systems/der Anwendung sowohl unter normalen als
	auch unter Spitzenlastbedingungen überprüft werden.
##### Performance Testing
	Bei dieser Art von Tests werden die Geschwindigkeits-,
	Skalierbarkeits- und/oder Stabilitätseigenschaften des zu
	prüfenden Systems oder der zu prüfenden Anwendung ermittelt
	oder validiert.
##### Weitere...
	- End-to-End Testing
		- Wertvoll, aber manchmal schwierig zu automatisieren
	- Unit-Tests
	- Integrationstests

#### Automatisiertes Testen vs Manuelles Testen
![[Pasted image 20230116092629.png]]

#### Bekannte Testautomatisierungsframeworks
- HP QTP (Quick Test Professional) / UFT (Unified 01
- Silk Test
- Selenium
- Test Complete
- ....

#### Security Testing
[PDF Link](DevSecOps_Kapitel3.pdf#page=10)
##### Auswahl an Techniken
- Vulnerability Scanning
- Password Virus
- Cracking Detect
- Penetration Check ers
- DOS
- ....

##### Hauptschutzziele des Security Testing
1. Availability
2. Integrity
3. Authorization
4. Confidentiality
5. Authentication
6. Non Repudiation -> Nicht-Abstreitbarkeit

##### Security Testing in den unterschiedlichen Phasen des SDLC
![[Pasted image 20230116095850.png]]

#### SAST 
[PDF Link](DevSecOps_Kapitel3.pdf#page=13)
##### Definition
- Static Application Security Testing (SAST), oder statische Analyse, ist eine __Testmethode, die den Quellcode analysiert, um Sicherheitslücken zu finden.__
	- SAST scannt die Anwendung, __bevor der Code kompiliert wurde__
	- White Box Testing
	- Hilfreich für Entwickler, um Schwachstellen frühzeitig zu erkennen und zu beheben
##### Ablauf und Prozess
![[Pasted image 20230116100756.png]]

##### Vor- und Nachteile
- Je früher eine Schwachstelle im Softwareentwicklungsprozess behoben wird:
	- Weniger Kosten und Ressourcen (Dev/Test/Production – 1/10/100)
- __Großer Vorteil:__ 100 % Source Code Abdeckung möglich + Line of Code Information
- __Nachteil__, frühe Integration im SDLC: Sehr viele False-Positives (Bugs, Schwachstellen)
- __Wichtig: Tooling-Auswahl + Grad der Automatisierung + klarer Prozess sind entscheidend!__

##### Integration von SAST in CI/CD
- So früh wie möglich -> __Alle Repos, Alle Branches, Jeden Commit scannen__ - z.B. Jenkins + SonarQube – oder sogar noch vorher in der IDE
- Prozess definieren – Wer ist wofür zuständig – Stakeholder einbinden - Fehlerkultur etablieren
- __False positives abarbeiten__
	- Security Know-how
	- Programmiersprachenerfahrung • Anwendungsexpertise 
	- Domänenwissen 
	- Deploymentkenntnisse 
	- Tooling-Wahl wichtig und entscheidend

#### DAST - dynamic application security testing 
Dynamic Application Security Testing (DAST) ist ein Verfahren, das aktiv laufende Anwendungen mit __Penetrationstests__ untersucht, um mögliche Sicherheitslücken zu erkennen.

#### Snyk
[PDF Link](DevSecOps_Kapitel3.pdf#page=17)

#### Zusammenfassung K3
[PDF Link](DevSecOps_Kapitel3.pdf#page=47)
- _Security Testing_ ist essentiell, um die Großzahl an Fehlern zu finden, bevor die Anwendung an den Kunden geht und Schwachstellen ausgenutzt werden
- _SAST_ ist ein wichtiges Security-Werkzeug für die statische Codeanalyse im frühen SDLC-Stadium 
- __Hauptziel ist das Auffinden von Schwachstellen und Bugs in Softwareanwendungen __
- Je höher die Softwarequalität, desto sicherer ist die Softwareanwendung
- Klarer Prozess und Automatisierung _(CI/CD)_ sind wichtig 
- _SAST_ sollte durch weitere automatisierte Testmethoden wie _DAST_ ergänzt werden 
- _SAST_ ist aber nur ein Baustein im DevSecOps-Modell

### Kapitel 4
[PDF Link](DevSecOps_Kapitel4.pdf)

#### CI/CD
[PDF Link](Chapter_3_WAS.pdf#page=2)
![[Pasted image 20230116104119.png]]

##### Continuous Integration vs Continuous Delivery vs Continuous Deployment
![[Pasted image 20230116104158.png]]

##### Continuous Delivery vs Continuous Deployment
![[Pasted image 20230116104319.png]]

#### Application Deployment
[PDF Link](Chapter_3_WAS.pdf#page=6)
##### Herausforderungen
1. Verlorene Einnahmen
2. Kundenzufriedenheit
3. Hohe wiederkehrende Arbeitslast
4. Sanktionen, Firmenwertverlust

##### Strategien
- Recreate
	- Wenn Softwareversion A beendet ist, wird Version B ausgerollt.
- Ramped
	- Die Softwareversion B wird langsam ausgerollt und ersetzt Softwareversion A.
- Blue/Green
	- Die Softwareversion B wird __parallel__ zu Softwareversion A ausgerollt. Im Anschluss wird der __Netzwerkverkehr auf B umgeleitet.__
- Shadow
	- Softwareversion B erhält ebenfalls den Produktiv-Netzwerkverkehr neben Softwareversion A, aber Antworten kommen von A
- A/B testing
	- Softwareversion B wird für eine __Untergruppe von Benutzern unter bestimmten Bedingungen freigegeben.__
- Canary
	- Ähnlich wie __Blue/Green,__ aber Softwareversion B wird für eine __Untergruppe von Benutzern freigegeben__, danach erfolgt das vollständige Rollout.

#### Cloud-Deploymentmodelle
[PDF Link](Chapter_3_WAS.pdf#page=10)
##### IaaS vs PaaS vs SaaS
![[Pasted image 20230116105720.png|500]]

##### Anwendungsszenarios
![[Pasted image 20230116110201.png]]

#### Microservice Architektur
[PDF Link](Chapter_3_WAS.pdf#page=14)
Eine Microservice-Architektur ist ein Software-Design-Stil, bei dem eine Anwendung als Sammlung von __kleinen, autonomen Diensten implementiert__ wird, die miteinander kommunizieren und ihre Aufgaben __unabhängig voneinander ausführen.__ 
Jeder Microservice ist als __eigenständige Komponente__ implementiert und stellt __eine Funktion bereit__. Die Kommunikation zwischen den Diensten erfolgt über leichtgewichtige Schnittstellen. Dieser Ansatz bietet eine Reihe von Vorteilen, wie Skalierbarkeit, Flexibilität, Unabhängigkeit und die Möglichkeit, schneller an neue Anforderungen anzupassen.

#### Serverless Architekturen
[PDF Link](Chapter_3_WAS.pdf#page=20)
![[Pasted image 20230116113847.png]]

#### Containerisierung
[PDF Link](Chapter_3_WAS.pdf#page=25)
Ähnlich wie VMs sind Container eine Art „Behälter“ für Anwendungen, in dem diese laufen können.


##### OCI Standards
- Die Open Container Initiative (OCI) wurde gegründet, um Standards für Container-Images und - Laufzeiten zu definieren.
	- In Anlehnung an Docker Mechanismen und Techniken – viele Gemeinsamkeiten dementsprechend
	- Ziel der OCI: Standards für Container zu definieren und gleichzeitig kompatibel mit Docker bleiben
	- OCI-Spezifikationen: Image-Format
![[Pasted image 20230116111142.png]]

##### Bedrohungen und Angriffe
![[Pasted image 20230116111220.png]]

##### Container Images
- Basis für Container bilden sogenannte __Images,__ vereinfacht ausgedrückt:
	- Datei durch die die Installation und das Updaten einer Software wegfällt
	- Image-Inhalt: Root Filesystem und Konfigurationen
	- Beinhaltet alle Komponenten, um eine Anwendung plattformunabhängig auszuführen 
	- Image kann einfach auf ein anderes System übertragen werden (Kopieren, aus Registry laden) 
	- Container lässt sich aus dem Image in entsprechender ContainerUmgebung (z.B. Docker) starten
- Verfügbar gemacht werden Images über eine __Container Registry__
	- Dort werden sie gespeichert, verwaltet und bereitstellt
	- Die bekannteste öffentliche Registry ist _Docker_ Hub1 – viele frei verfügbare Images

#### Container-Architekturen 
[PDF Link](Chapter_3_WAS.pdf#page=37)
![[Pasted image 20230116112213.png|400]]
##### setuid
The setuid (set user ID) bit is a special type of file permission available in Linux and other Unix-like operating systems. It allows executable files to be run with the permissions of the file's owner instead of the user running it. 
__A wrongly set setuid can have serious consequences
(e.g. with bash) - everyone who executes a command via bash would have root privileges__

##### Linux Capabilities
Linux capabilities are a __set of privileges that can be independently assigned to processes running on a Linux__ system. It allows for more granular control over what processes can do than traditional Unix permissions, such as read, write, and execute. For example, rather than granting all privileges associated with the root user to a process, Linux capabilities __allow the process to be given only certain specific privileges necessary to accomplish its task.__ This helps with security by reducing the attack surface of a system and limiting what a malicious process can do if it is compromised.

##### Cgroups
Cgroups (Control Groups) is a Linux kernel __feature that limits, accounts for, and isolates the resources used by certain processes.__ It allows system administrators to allocate resources—such as CPU time, system memory, network bandwidth, or combinations of these—among user-defined groups of tasks (processes) running on a system. Cgroups are commonly used in virtualization, containerization and clustering environments.

##### chroot
Chroot is a command that can __change the root directory in Linux or UNIX.__ It is used to __isolate a process and its children from all other processes on the system,__ providing an environment where a process can run without access to any files outside of the chroot jail. This can be used for security purposes, as well as for testing and debugging.

##### Namespaces#
Limit namespaces, what is visible to a container is visible during execution

#### Best Practices 
[PDF Link](Chapter_3_WAS.pdf#page=57)
##### Containerdesign
- Idealerweise __Service pro Container__ – Stichwort Microarchitecture
- __Keine Nutzdaten (persistente Daten) im Container speichern__
	- Container sind standardmäßig _„Immutable Components“_
	- Beim Beenden oder neuem Deployment sind alle zur Laufzeit erzeugten Daten weg 
	- Für Nutzdaten wird ein externes, persistentes Volume verwendet (docker –v)
- __Containerverwaltung und Konfiguration sollte über Automatisierungstools erfolgen__
	- Jenkins 
	- Terraform 
	- Ansible
##### Dockerfile
- Base Image
	- Erste Zeile des Dockerfiles definiert das Base Image _(FROM-Anweisung)_
	- Base Image sollte aus einer __vertrauenswürdigen Registry__ stammen 
	- Je __kleiner__ das Basis-Image, desto geringer Wahrscheinlichkeit für unnötigen oder schadhaften Code
- Image Tags
	- __Vorsicht mit Tags – besser den SHA256-digest zur Prüfung verwenden__
- Multi-Stage Builds1 – _Ziel: unnötige Inhalte im endgültigen Image eliminieren_
	- Technik: __Verschiedene Abschnitte__ im Dockerfile, die jeweils auf ein __anderes Base Image verweisen__ 
	- Erste Stage: alle Pakete, Libs und die Toolchain – nicht zur Laufzeit benötigt
	- Zweite Stage: Umgebung vorbereiten, Dateien kopieren, etc • Dritte Stage: Golden Image erstellen mit Lean Base Image
##### Betrieb von Container-Anwendungen
![[Pasted image 20230116113513.png]]

#### Zusammenfassung K4
[PDF Link](DevSecOps_Kapitel4.pdf#page=59)
- CI/CD ist das Kernstück von DevSecOps und ist letztlich das Automatisierungsframework für alle IT-Sicherheitsaktivitäten
- Es gibt eine Vielzahl von Deployment-Optionen und die Tendenz geht Richtung Cloud-Deployment, insbesondere Container-basierte Deployments 
- Container-Sicherheit ist ein komplexes Thema und muss Stück für Stück umgesetzt werden 
- Hier haben wir die wichtigsten Techniken zur Container-Absicherung beim Erstellen von Images als auch beim Betrieb von Containern kennengelernt
