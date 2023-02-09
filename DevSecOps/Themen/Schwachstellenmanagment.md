---
cards-deck: DevSecOps::Themen
---

## CVSS 

### Base Metrics
CVSS (Common Vulnerability Scoring System) v3.1 is a standardized method for **evaluating the severity of a security vulnerability.** The Base Metrics are the core component of the CVSS score, and they provide a quantitative representation of the underlying characteristics of a vulnerability. The Base Metrics are divided into three groups:

1.  _Attack Vector (AV):_::Gibt an, **wie einfach der Zugriff auf die verwundbare Komponente** aus der Sicht eines Angreifers ist, z. B. ob für den Angriff lokaler oder Netzwerkzugriff erforderlich ist.
^1675871762092
    
2.  _Attack Complexity (AC):_::Beschreibt die Komplexität des Angriffs, der erforderlich ist, um die Schwachstelle auszunutzen, z. B. ob spezielle Kenntnisse oder Fähigkeiten erforderlich sind.
^1675871762096
    
3. _Privileges Required (PR):_::Bezieht sich auf die Zugriffsebene, die zur Durchführung des Angriffs erforderlich ist, z. B. ob der Angreifer Administrator- oder Root-Rechte benötigt.
^1675871762098
    
4.  _User Interaction (UI):_::Gibt an, ob die Schwachstelle ohne Benutzerinteraktion ausgenutzt werden kann, d. h. ob der Benutzer eine bestimmte Aktion ausführen muss, damit der Angriff erfolgreich ist. 
^1675871762100
    
5.  _Scope (S):_::Gibt an, ob die Schwachstelle nur Ressourcen innerhalb des Geltungsbereichs der verwundbaren Komponente betrifft oder ob sie sich auch auf Ressourcen außerhalb der verwundbaren Komponente auswirken kann. U -> Unchanged / C -> Changed
^1675871762102
    
6.  _Confidentiality Impact (C):_::Bezieht sich auf die Auswirkungen auf die Vertraulichkeit von Daten, z. B. ob der Angreifer auf sensible Informationen zugreifen oder sie verändern kann.
^1675871762104
    
7.  _Integrity Impact (I):_::Beschreibt die Auswirkungen auf die Integrität der Daten, z. B. ob der Angreifer Daten verändern oder zerstören kann.
^1675871762105
    
8.  _Availability Impact (A):_::Bezieht sich auf die Auswirkungen auf die Verfügbarkeit der verwundbaren Komponente, z. B. ob der Angreifer einen Denial-of-Service-Zustand verursachen kann.
^1675871762107

### Privileges Required (PR)

| Metric Value | Description |
| --- | --- |
| None (N) | The attacker is unauthorized prior to attack, and therefore does not require any access to settings or files of the vulnerable system to carry out an attack. |
| Low (L) | The attacker requires privileges that provide basic user capabilities that could normally affect only settings and files owned by a user. Alternatively, an attacker with Low privileges has the ability to access only non-sensitive resources. |
| High (H) | The attacker requires privileges that provide significant (e.g., administrative) control over the vulnerable component allowing access to component-wide settings and files. |

### Qualitative Severity Rating Scale #card 

| Rating   | CVSS Score |
| -------- | ---------- |
| None     | 0.0        |
| Low      | 0.1 - 3.9  |
| Medium   | 4.0 - 6.9  |
| High     | 7.0 - 8.9  |
| Critical | 9.0 - 10.0 |
|          |            |
^1675869551095


## Aufgabe 3 – Schwachstellenmanagement

1.  Tipp: Schauen Sie die Übung 4 zum Thema Schwachstellenmanagement im Detail an.
    
2.  Beispielaufgabe: Analysieren Sie folgendes Szenario, indem Sie die Teilaufgaben a) bis d) lösen.
    
    Die Heartbleed Schwachstelle in der OpenSSL Bibliothek wird nach ca. 10 Jahren immer noch ausgenutzt. 2014 wurde Heartbleed als schwerwiegende Schwachstelle identifiziert und behoben. Im November letzten Jahres ist eine neue Schwachstelle bekannt geworden, die ebenfalls kritisch ist. Für Heartbleed existieren eine Reihe von CVEs und CVSSv3.1- Scores von 9 und höher. Doch auch heute existieren noch immer ungepatchte Systeme weltweit.
    
    1.  Welches Rating ergibt ein CVSSv3.1 von 9 und höher?
	    - Critical 
	
    2.  Gegeben sei folgender CVSS-Vektor: _CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N_ Analysieren Sie **alle Bestandteile** des CVSS-Vektors und geben Sie abschließend eine **High-Level-Beschreibung der zugehörigen Schwachstelle.**
	    - CVSS:3.1
		    - Common Vulnerability Scoring Systems Version -> 3.1
	    - AV
		    - Angriffsvektor -> Netzwerk
	    - AC
		    - Angriffskomplexität -> Niedrig
	    - PR
		    - Benötigte Privilegien -> None
	    - UI
		    - User Interaktion -> None -> keine Interaktion nötig
	    - S
		    - Scope -> Unchanged -> keine Auswirkungen auf andere Komponeneten
	    - C
		    - Auswirkung auf vertrauliche Daten -> Hoch -> Daten könnten entschlüsselt werden
	    - I
		    - Einfluss auf integrität -> None -> keine manipulation der Daten möglich
	    - A
		    - Einfluss auf Verfügbarkeit -> N -> keine Denial-of-Service Angriff möglich
		    
	    Zusammenfassend handelt es sich um einen Netzwerk-basierten Angriff auf die Vertraulich eines Systems, welcher insgesamt relativ leicht durchgeführt werden kann, aber zumindest keine Auswirkungen auf weitere Systeme hat.
	    
    4.  Welche Prozess-Schritte sollten insbesondere für kritische Schwachstellen durchgeführt werden?
	    - Überwachung und Überprüfung von Sicherheitsmeldungen und Patches von Software-Herstellern.
	    - Installation von Patches und Upgrades für betroffene Systeme.
	    - Überwachung von Netzwerken auf ungewöhnliche Aktivitäten.
	    - Überprüfung von Backups und Business Continuity-Plänen.
	    - Schulung von Benutzern über sichere Praktiken und erhöhte Wachsamkeit gegenüber sozialen Angriffen. 