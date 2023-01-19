cards-deck: DevSecOps::summarys
---

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
	    
## Kapitel 3 - Security Testing & SAST
[PDF Link](DevSecOps_Kapitel3.pdf)
__Hauptziele des Security Testing__
    -   Testen ist eine wichtige Tätigkeit im _SDLC_, insbesondere bevor der Kunde die Software erhält
    -   Security Testing legt einen Fokus auf Fehler, die als Schwachstellen ausgenutzt werden können
    - _Hier Fokus auf automatisierte Tests_
        
-   __Hauptziele des Static Application Security Testing__
    -   Fehler im Source Code und potentielle _Schwachstellen frühzeitig finden (während der Entwicklung)_
    -   _Automatisierte Tools unterstützen die Entwickler_ und machen eine sichere Entwicklung erst möglich
^1673868798945


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

##### Regression Testing #card
	eignen sich für automatisierte Tests, da der
	Code häufig geändert wird und die menschliche
	Fähigkeit zur rechtzeitigen und qualitativen
	Durchführung von Tests überfordert ist.
	^1673869588619
##### Load testing #card
	Es soll überprüft werden, ob das System/die
	Anwendung die erwartete Anzahl von
	Transaktionen bewältigen kann, und das
	Verhalten des Systems/der Anwendung sowohl
	unter normalen als auch unter 
	Spitzenlastbedingungen überprüft werden.
	^1673869699916
##### Performance Testing #card
	Bei dieser Art von Tests werden die 
	Geschwindigkeits-, Skalierbarkeits- und/oder 
	Stabilitätseigenschaften des zu
	prüfenden Systems oder der zu prüfenden
	Anwendung ermittelt oder validiert.
	^1673869771390
##### Weitere...
	- End-to-End Testing
	- Wertvoll, aber manchmal schwierig zu 
		automatisieren
	- Unit-Tests
	- Integrationstests




#### Automatisiertes Testen vs Manuelles Testen
![[Pasted image 20230116092629.png]]

#### Bekannte Testautomatisierungsframeworks #card
- HP QTP (Quick Test Professional) / UFT (Unified 01
- Silk Test
- Selenium
- Test Complete
- ....
^1673869997467

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

## Kapitel 4
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

