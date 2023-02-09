### Shift- Left:
https://www.aquasec.com/cloud-native-academy/devsecops/shift-left-devops/
- Aspekte: Shift left security, Shift left testing, Automatisierung von IT- Sicherheitsprozessen, IT-Sicherheitstraining, Security by design.
- bezieht sich auf die Bemühungen eines DevOps-Teams, die Anwendungssicherheit in den frühesten Stadien des Entwicklungslebenszyklus zu gewährleisten, als Teil eines Organisationsmusters, das als DevSecOps (Zusammenarbeit zwischen Entwicklung, Sicherheit und Betrieb) bekannt ist.
### Stride Modell
- Spoofing:
	- IT-Sicherheitsziel: Authentication
	- Angreifern geben eine falsche Identität vor
- Tampering
	- IT-Sicherheitsziel: Integrity
	- Angreifer können unbefugt Daten verändern
- Repudiation (Ablehnung):
	- IT-Sicherheitsziel: Non-Repudiation
	- Es kann geleugnet werden, für einen Angriff verantwortlich zu sein
- Information Disclosure:
	- IT-Sicherheitsziel: Confidentiality (Vertraulichkeit)
	- Angreifer können schützenswerte Informationen stehlen
- Denial of Service:
	- IT-Sicherheitsziel: Availibility
	- Angriffe, die ein System unbrauchbar machen
- Elevation of Privilege:
	- IT-Sicherheitsziel: Authorization
	- Angreifer kann etwas tun, wozu er nich befugt ist (fehlende Berechtigungen)

### CVSS-Vektor
https://www.first.org/cvss/v3.1/specification-document
- AV - Attack Vector (Network, Adjacent, Local, Physical)
- AC - Attack Complexity: None, Low, High
- PR - Priviliges: None, Low, High
- UI - User Interaction: None, Required
- S - Scope: Changed, Unchanged
- C - Confidentiality: None, Low, High
- I - Integrity: None, Low, High
- A - Availability: None, Low, High

### Isolationskonzepte:
- setuid
	- ermöglicht erhöhte Berechtigungen für einen Befehl
	- falsch gesetzte setuid kann unbemerkt root-Berechtigung dem Attacker geben
	- Gegenmaßnahme:
		- --security-opt=no-new-priviliges
- Linux Capabilities:
	- ermöglicht feingranular die Einschränkungen möglicher Kernel-Interaktionen
	- --cap-add and --cap-drop zur Verwaltung von Capabilities
	- seccomp fasst eingeschränkte und erlaubte Systemaufrufe in Profilen zusammen und begränzt die Anzahl von Syscalls, die aus dem Container aufgerufen werden können
- Cgroups:
	- Common Groups limitieren Ressourcen, wie Speicher, CPU etc., die von Container genutzt werden können
	- beim Container Start wird ein neues Set an cgroups angelegt
- chroot:
	- definiert den Ausgangspunkt im Filesystem für einen Container ( root directory)
- Namespaces:
	- limitieren was für einen Container während der Ausführung sichtbar ist

- Welche Prozess-Schritte sollten insbesondere für kritische Schwachstellen durchgeführt werden?
1. So schnell wie möglich Informationen über Schwachstelle sammeln.
2.  CVE analysieren (Fehler und Risiko prüfen).
3.  Alle betroffenen Systeme identifizieren.
4.  Feststellen, ob Systeme betroffen sind.
5.  Hotfixplanung.
6.  Prüfen, ob Hotfix erfolgreich war (Schritt 4 wiederholen und ggf. Schritt 5)