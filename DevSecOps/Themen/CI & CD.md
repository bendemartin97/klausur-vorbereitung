---
cards-deck: DevSecOps::Themen
---

## Github Actions

You can configure a GitHub Actions _workflow_ to be triggered when an _event_ occurs in your repository, such as a pull request being opened or an issue being created. Your workflow contains one or more _jobs_ which can run in sequential order or in parallel. Each job will run inside its own virtual machine _runner_, or inside a container, and has one or more _steps_ that either run a script that you define or run an _action_, which is a reusable extension that can simplify your workflow.

### Workflows
A workflow is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule.

Workflows are defined in the `.github/workflows` directory in a repository, and a repository can have multiple workflows, each of which can perform a different set of tasks. For example, you can have one workflow to build and test pull requests, another workflow to deploy your application every time a release is created, and still another workflow that adds a label every time someone opens a new issue.

### Events
An event is a specific activity in a repository that triggers a workflow run. For example, activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository. You can also trigger a workflow to run on a schedule, by [posting to a REST API](https://docs.github.com/en/rest/reference/repos#create-a-repository-dispatch-event), or manually.

### Jobs
A job is a set of _steps_ in a workflow that execute on the same runner. Each step is either a shell script that will be executed, or an _action_ that will be run. Steps are executed in order and are dependent on each other. Since each step is executed on the same runner, you can share data from one step to another. For example, you can have a step that builds your application followed by a step that tests the application that was built.

You can configure a job's dependencies with other jobs; by default, jobs have no dependencies and run in parallel with each other. When a job takes a dependency on another job, it will wait for the dependent job to complete before it can run. For example, you may have multiple build jobs for different architectures that have no dependencies, and a packaging job that is dependent on those jobs. The build jobs will run in parallel, and when they have all completed successfully, the packaging job will run.

### Action
An _action_ is a custom application for the GitHub Actions platform that performs a complex but frequently repeated task. Use an action to help reduce the amount of repetitive code that you write in your workflow files. An action can pull your git repository from GitHub, set up the correct toolchain for your build environment, or set up the authentication to your cloud provider.

You can write your own actions, or you can find actions to use in your workflows in the GitHub Marketplace.

### Runner
A runner is a server that runs your workflows when they're triggered. Each runner can run a single job at a time. GitHub provides Ubuntu Linux, Microsoft Windows, and macOS runners to run your workflows; each workflow run executes in a fresh, newly-provisioned virtual machine. GitHub also offers larger runners, which are available in larger configurations. For more information, see "[Using larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/using-larger-runners)." If you need a different operating system or require a specific hardware configuration, you can host your own runners. For more information about self-hosted runners, see "[Hosting your own runners](https://docs.github.com/en/actions/hosting-your-own-runners)."

#### Flashcards
Was ist ein Github Actions-Workflow?:::Ein konfigurierbarer automatisierter Prozess, der einen oder mehrere Jobs ausführt, die durch ein Ereignis in einem Repository oder manuell oder nach einem festgelegten Zeitplan ausgelöst werden, definiert durch eine YAML-Datei im Verzeichnis '.github/workflows' in einem Repository.
^1675869940189

Was sind Github Actions-Ereignisse?:::Bestimmte Aktivitäten in einem Repository, die einen Workflow-Lauf auslösen, wie z. B. eine Pull-Anfrage, ein Issue, ein Commit, ein Zeitplan, eine REST-API oder manuell.  
^1675869940192
  
Was sind Github Actions Jobs?:::Eine Reihe von Schritten in einem Workflow, die auf demselben Runner laufen, aus Shell-Skripten oder Aktionen bestehen und in der richtigen Reihenfolge und in Abhängigkeit voneinander ausgeführt werden. Jobs können so konfiguriert werden, dass sie von anderen Jobs abhängen.  
^1675869940194
  
Was sind Github Actions Aktionen?:::Eine benutzerdefinierte Anwendung für die Github Actions Plattform, die eine komplexe, häufig wiederkehrende Aufgabe ausführt und dazu beiträgt, sich wiederholenden Code in Workflow-Dateien zu reduzieren. Actions können selbst geschrieben oder auf dem Github Marketplace gefunden werden.  
^1675869940195
  
Was sind Github Actions-Runner?:::Ein Server, der Workflows ausführt, wenn sie ausgelöst werden. Er bietet Ubuntu-Linux-, Microsoft-Windows- und macOS-Runner oder kann für verschiedene Betriebssysteme oder Hardwarekonfigurationen selbst gehostet werden.  
^1675869940197

## Aufgabe #1
1.  Tipp: Wiederholen Sie das Thema GitHub Actions (Praktikum 2 und 3), um mit den Begrifflichkeiten vertraut zu sein und GitHub Workflows analysieren zu können.
    
2.  Beispielaufgabe: Analysieren Sie folgenden GitHub Workflow, indem Sie die Teilaufgaben a) bis c) lösen.

![[Pasted image 20230206112334.png]]
	1.  Analysieren Sie die Funktion dieses Workflows und beschreiben Sie wofür dieser Workflow genutzt werden kann.
		-  Der Workflow entschlüsselt ein geheimes Passwort mit Hilfe einer Passphrase ineinem Shell-Skript, welche als verschlüsselte Variable im Repository hinterlegt ist und gibt dieses Secret dann via „cat“ im Terminal aus. Der Job wird bei einem beliebigen Push im Repository ausgelöst, macht einen Checkout im Repository und es wird Ubuntu als GitHub-Runner verwendet. Der Workflow kann genutzt werden, um ein Datenbankpasswort zu entschlüsseln und sich dann zur Datenbank mit dem Passwort zu verbinden.
	1.  In welchem Format werden GitHub Workflows gespeichert?
		- YAML 
	3.  Sehen Sie ein IT-Sicherheitsproblem bei der Verwendung dieses Workflows?
	    Begründen Sie ihre Antwort.
	    - Ja, die Ausgabe des Secrets im Terminal sollte nur zu Testzwecken verwendet
	      werden und in der Produktivumgebung entfernt werden, da das Secret dann z.B. in Log-Dateien der Workflow-Ausführungen preisgegeben wird.

## Aufgabe #2

Arbeiten Sie sich in das Thema GitHub Actions ein: https://docs.github.com/en/actions/learn-github-actions  
Zur Abnahme sollten Sie folgende Fragen beantworten können:

1.  Erläutern Sie die Hauptbestandsteile von GitHub Actions.
    - Events und Jobs
	    - Events: trigger der ausführung des Workflows. z.B. push
	    - Jobs: Jobs laufen auf virtuellen Maschinen (runner) und beinhalten steps, die ausgeführt werden.
1.  Wie können Sie sich den Status eines gestarteten Workflows anschauen?
    
3.  Analysieren Sie den SonarCloud-Integrations-Workflow in der build.yml und erklären
    
    Sie zeilenweise was passiert.
    
4.  Was ist der genaue Command der SonarCloud GitHub Action, die bei jedem Commit
    
    in das Repository ausgeführt wird, um eine neue Analyse des Codes in SonarCloud zu erhalten?
    

5.  Wofür dient der GitHub Token bei GitHub Actions und welche Berechtigungen hat man mit diesem Token?
    
6.  Erstellen Sie einen neuen Workflow in ihrem Repository, der bei einem neu angelegten Issue in ihrem Repository das Label „NEW ISSUE“ setzt, sofern noch kein Label durch den Ticket-Ersteller vergeben wurde.  
    Nutzen Sie hierfür die GitHub Action „Simple Issue Labeler“ aus dem Marketplace in Version 1.0.4 (siehe https://github.com/marketplace/actions/simple-issue-labeler)
