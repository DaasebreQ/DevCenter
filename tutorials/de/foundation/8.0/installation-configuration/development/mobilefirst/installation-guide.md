---
layout: tutorial
title: Installationshandbuch für Workstations
breadcrumb_title: Installationshandbuch
weight: 1
---
<!-- NLS_CHARSET=UTF-8 -->
## Übersicht
{: #overview }
Folgen Sie diesem Installationshandbuch, wenn Sie Ihre Workstation für die Entwicklung mit der {{ site.data.keys.product }} einrichten.

## DevKit Installer
{: #devkit-installer }
Der [{{ site.data.keys.mf_dev_kit }} Installer]({{site.baseurl}}/tutorials/en/foundation/8.0/installation-configuration/development/mobilefirst)
installiert {{ site.data.keys.mf_server }}, eine Datenbank und eine Laufzeit auf Ihrem Entwicklersystem. Nach der Installation sind die genannten Komponenten sofort einsatzfähig.   

**Voraussetzungen:**  
Für den Installer muss Java installiert sein. 

1. [Installieren Sie die Oracle-JRE](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html).

2. Fügen Sie eine Variable `JAVA_HOME` hinzu, die auf die JRE zeigt. 

    *Mac und Linux:* Bearbeiten Sie wie folgt Ihre Datei **~/.bash_profile**:

    ```bash
    #### ORACLE JAVA
    export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_73.jdk/Contents/Home"
    ```

    *Windows:*  
    [Folgen Sie dieser Anleitung](https://confluence.atlassian.com/doc/setting-the-java_home-variable-in-windows-8895.html).

    [Bekanntes Problem](#known-issue-win)

## Installation
{: #installation }
Rufen Sie den DevKit Installer von der [Downloadseite]({{site.baseurl}}/downloads/) ab und folgen Sie den angezeigten Anweisungen. 

![DevKit Installer](devkit-installer.png)

### Server starten und stoppen
{: #starting-and-stopping-the-server }
Öffnen Sie ein Befehlszeilenfenster und navigieren Sie zur Position des Ordners mit den extrahierten Dateien. 

*Mac und Linux:*  

* Server starten: `./run.sh -bg`
* Server stoppen: `./stop.sh`

*Windows:*  

* Server starten: `./run.cmd -bg`
* Server stoppen: `./stop.cmd`

### Zugriff auf die {{ site.data.keys.mf_console }}
{: #accessing-the-mobilefirst-operations-console }
Sie können
wie folgt auf die [{{ site.data.keys.mf_console }}]({{site.baseurl}}/tutorials/en/foundation/8.0/product-overview/components/console/) zugreifen: 

* Führen Sie in der Befehlszeile den Befehl `mfpdev server console` aus. 
* Rufen Sie in einem Browser [http://localhost:9080/mfpconsole](http://localhost:9080/mfpconsole) auf. 

![Konsole]({{site.baseurl}}/tutorials/en/foundation/8.0/product-overview/components/console/dashboard.png)

## {{ site.data.keys.mf_cli }}
{: #mobilefirst-cli }
Die
[{{ site.data.keys.mf_cli }}]({{site.baseurl}}/tutorials/en/foundation/8.0/application-development/using-mobilefirst-cli-to-manage-mobilefirst-artifacts) ist eine Befehlszeilenschnittstelle, über die Sie
Anwendungen in {{ site.data.keys.mf_server }} registrieren, eine Anwendung per Pull/Push vom oder zum {{ site.data.keys.mf_server }} übertragen,
Java- und JavaScript-Adapter erstellen, mehrere lokale und ferne Server verwalten,
ein Update für Liveanwendungen mittels direkter Aktualisierung durchführen und viele weitere Schritte ausführen können. 

**Voraussetzungen:**  
1. Node.js und NPM sind Voraussetzungen für die Installation der {{ site.data.keys.mf_cli }}.  
 Sie müssen [NodeJS Version 6.11.1](https://nodejs.org/download/release/v6.11.1/) und NPM Version 3.10.10 herunterladen und installieren. Für die MobileFirst-CLI ab Version 8.0.2018100112 können Sie Node Version 8.x oder Version 10.x verwenden.

 Überprüfen Sie die Installation. Öffnen Sie ein Befehlszeilenfenster und führen Sie den Befehl `node -v` aus.

2. Für einige CLI-Befehle ist Maven erforderlich. Dies gilt beispielsweise für Befehle zum Erstellen und Implementieren von Adaptern und Adapterbuilds. Die Installationsanweisungen finden Sie im nächsten Abschnitt. 

### Installation der {{ site.data.keys.mf_cli }}
{: #installation-cli }
Öffnen Sie ein Terminal und führen Sie den Befehl `npm install -g mfpdev-cli` aus.  

*Mac und Linux:* Möglicherweise müssen Sie den Befehl mit `sudo` ausführen.  
Informieren Sie sich über die Möglichkeiten der [Korrektur von NPM-Berechtigungen](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

Überprüfen Sie die Installation. Öffnen Sie ein Befehlszeilenfenster und führen Sie den Befehl `mfpdev -v` oder `mfpdev help` aus.

![Konsole](mfpdev-cli.png)

## Adapter und Sicherheitsüberprüfungen
{: #adapters-and-security-checks }
[Adapter]({{site.baseurl}}/tutorials/en/foundation/8.0/adapters)
und [Sicherheitsüberprüfungen]({{site.baseurl}}/tutorials/en/foundation/8.0/authentication-and-security)
sind der Königsweg für das Einbringen der Authentifizierung und anderer Sicherheitsebenen in Ihre Anwendung. 

**Voraussetzungen:**  
Vor dem Erstellen von Adaptern und Sicherheitsüberprüfungen müssen Sie Apache Maven konfigurieren.   

1. [Laden Sie die ZIP-Datei für Apache Maven herunter](https://maven.apache.org/download.cgi). 
2. Fügen Sie eine Variable `MVN_PATH` hinzu, die auf den Maven-Ordner zeigt. 

   *Mac und Linux:* Bearbeiten Sie wie folgt Ihre Datei **~/.bash_profile**:

    ```bash
    #### Apache Maven
    export MVN_PATH="/usr/local/bin"
    ```

    *Windows:*

    [Folgen Sie dieser Anleitung](http://crunchify.com/how-to-setupinstall-maven-classpath-variable-on-windows-7/).
    
    Überprüfen Sie die Installation, indem Sie Folgendes ausführen:
    ```bash
    mvn -v
    ```


### Verwendung
{: #usage }
Jetzt, da Apache Maven installiert ist, können Sie Adapter mit Befehlen der Maven-Befehlszeilenschnittstelle
oder unter Verwendung der {{ site.data.keys.mf_cli }} erstellen.  
Weitere Informationen enthalten die Lernprogramme zu [Adaptern]({{site.baseurl}}/tutorials/en/foundation/8.0/adapters).

### Bekanntes Problem unter Windows
{: #known-issue-win}

Auf Windows-Maschinen mit Oracle Java 8 ab Update 60 erscheint bei Verwendung von MobileFirst-Installationsprogrammen, die die InstallAnywhere-Technologie nutzen, die folgende Fehlernachricht. 

![Startfehler](launch-error.png)

#### Fehlerursache
Von Oracle im Java-8-Update 60 eingeführte Änderungen führen dazu, dass mit InstallAnywhere erstellte Installationsprogramme fehlschlagen und den oben angegebenen Fehler melden. Dies liegt daran, dass InstallAnywhere in Oracle Java 8 ab Update 60 die Position der zuletzt auf der Windows-Maschine unterstützten JVM nicht automatisch ermitteln kann.

#### Korrektur des Fehlers

Sie können eine der folgenden Ansätze für die Lösung des Problems nutzen.

##### Ansatz 1

Weisen Sie InstallAnywhere an, unter Verwendung einer bestimmten JVM zu starten.
Starten Sie das Installationsprogramm über die Befehlszeile und geben Sie mit dem Parameter **LAX_VM** an, welche JVM InstallAnywhere für den Start verwenden soll.

![Parameter LAX_VM](lax-vm-parameter.png)

##### Ansatz 2

Da dieses Problem in Vorversionen von Java 8 Update 60 nich aufgetreten ist, können Sie Java 7 verwenden.
