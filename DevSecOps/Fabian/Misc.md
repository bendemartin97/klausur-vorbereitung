## Sonarcloud 
Tipp: Schauen Sie sich bei sonarcloud.io öffentliche Beispielprojekte (unter Explore) an, um mit der sonarcloud-Analyse von Bugs, Vulnerabilities, Hotspots und Code Smells vertraut zu sein (Praktikum 2). Programmiersprachen sind: Skriptsprachen (Javascript o.ä., Java, PHP). Es werden bekannte und gängige Schwachstellen vorkommen, die Sie analysieren müssen.

![[Pasted image 20230207105028.png | 400]]

The workflow for using SonarCloud is as follows:
1.  Code development: The developer writes code and commits changes to the code repository.
    
2.  Code analysis: The code is analyzed by SonarCloud as part of the build pipeline. During the analysis, SonarCloud checks the code for various quality issues, such as bugs, security vulnerabilities, and code smells.
    
3.  Results display: The results of the code analysis are displayed on the SonarCloud website. The results include a summary of the code quality, as well as detailed reports on specific issues found in the code.
    
4.  Issue triage: Developers review the results of the code analysis and decide which issues to address. They may choose to fix some issues immediately, while others may be deferred to a later time.
    
5.  Code improvement: Developers make changes to the code to address the identified issues.
    
6.  Repeat: The code is committed and the analysis process starts again. Over time, the code quality improves as the identified issues are fixed.


![[Pasted image 20230207105111.png]]

## Container-Sicherheit
Tipp: Schauen Sie sich insbesondere die Isolationskonzepte von Container-Architekturen nochmal an. Sie sollten die Konzepte verstanden haben und die wichtigsten Schlüsselwörter und Parameter kennen, um Container-Deployments analysieren und „härten“ zu können.

**Container-Isolation** bezieht sich auf die Trennung von Prozessen und Ressourcen innerhalb eines Containers, um sicherzustellen, dass sie unabhängig voneinander funktionieren. Hier sind einige der wichtigsten Konzepte und Begriffe:
1.  Namespaces: Teilen eines Host-Systems in verschiedene virtuellen Kontexten, um Prozesse und Ressourcen zu isolieren.
    
2.  Cgroups: Steuern und begrenzen von Ressourcen wie CPU, Speicher und Netzwerk für Container.
    
3.  Overlay-Netzwerke: Ermöglichen es, dass Container miteinander kommunizieren, aber ihre Netzwerke voneinander isoliert sind.
    
4.  Sicherheitsrichtlinien: Überwachen und Steuern von Zugriffen auf Systemressourcen und -dienste.
    
5.  Image Signing: Überprüfen der Integrität von Container-Images, um sicherzustellen, dass sie sicher sind, bevor sie ausgeführt werden.
    

Es ist wichtig, diese Konzepte zu verstehen, um Container-Deployments sicher und robust zu gestalten und potenzielle Angriffe oder Bedrohungen zu verhindern.


## Sichere Softwareentwicklung

#### Aufgabe 1 – Sichere Softwareentwicklung
1.  Beispielfrage: Nennen Sie alle fünf vorgestellten Aspekte des „Shift Left“-Paradigmas und erläutern Sie für zwei Aspekte, wie diese die IT-Sicherheit in der agilen Softwareentwicklung erhöhen.
	- „Shift Left“-Paradigma Aspekte: Shift left security, Shift left testing, Automatisierung von IT- Sicherheitsprozessen, IT-Sicherheitstraining, Security by design.  
	- Der „Security by design“ Aspekt ermöglicht die frühzeitige Architekturplanung mit IT- Sicherheitskomponenten und das Aufdecken von potentiellen Bedrohungen durch ein schlechtes Systemdesign in einer frühen Entwicklungsphase. Das IT-Sicherheitstraining ermöglicht den kontinuierlichen Aufbau von State-of-the-Art Wissen über IT- Sicherheitsmaßnahmen, aktuelle Bedrohungen und Best Practices in der sicheren Softwareentwicklung. 
    
2.  Tipp: Schauen Sie sich die vorgestellten Entwicklungsmodelle im Detail an und überlegen Sie sich, wie IT-Sicherheit in diesen Entwicklungsmodellen integriert werden kann.