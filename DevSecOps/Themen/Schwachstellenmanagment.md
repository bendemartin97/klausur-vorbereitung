---
cards-deck: DevSecOps::Themen
---

## CVSS 

### Base Metrics
CVSS (Common Vulnerability Scoring System) v3.1 is a standardized method for **evaluating the severity of a security vulnerability.** The Base Metrics are the core component of the CVSS score, and they provide a quantitative representation of the underlying characteristics of a vulnerability. The Base Metrics are divided into three groups:

1.  _Attack Vector (AV):_:::Gibt an, **wie einfach der Zugriff auf die verwundbare Komponente** aus der Sicht eines Angreifers ist, z. B. ob für den Angriff lokaler oder Netzwerkzugriff erforderlich ist.
^1675869405143
    
2.  _Attack Complexity (AC):_:::Beschreibt die Komplexität des Angriffs, der erforderlich ist, um die Schwachstelle auszunutzen, z. B. ob spezielle Kenntnisse oder Fähigkeiten erforderlich sind.
^1675869551066
    
3. _Privileges Required (PR):_:::Bezieht sich auf die Zugriffsebene, die zur Durchführung des Angriffs erforderlich ist, z. B. ob der Angreifer Administrator- oder Root-Rechte benötigt.
^1675869551072
    
4.  _User Interaction (UI):_:::Gibt an, ob die Schwachstelle ohne Benutzerinteraktion ausgenutzt werden kann, d. h. ob der Benutzer eine bestimmte Aktion ausführen muss, damit der Angriff erfolgreich ist.
^1675869551075
    
5.  _Scope (S):_:::Gibt an, ob die Schwachstelle nur Ressourcen innerhalb des Geltungsbereichs der verwundbaren Komponente betrifft oder ob sie sich auch auf Ressourcen außerhalb der verwundbaren Komponente auswirken kann.
^1675869551080
    
6.  _Confidentiality Impact (C):_:::Bezieht sich auf die Auswirkungen auf die Vertraulichkeit von Daten, z. B. ob der Angreifer auf sensible Informationen zugreifen oder sie verändern kann.
^1675869551083
    
7.  _Integrity Impact (I):_:::Beschreibt die Auswirkungen auf die Integrität der Daten, z. B. ob der Angreifer Daten verändern oder zerstören kann.
^1675869551088
    
8.  _Availability Impact (A):_:::Bezieht sich auf die Auswirkungen auf die Verfügbarkeit der verwundbaren Komponente, z. B. ob der Angreifer einen Denial-of-Service-Zustand verursachen kann.
^1675869551091

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
