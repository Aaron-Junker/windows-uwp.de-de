---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“
description: Die Windows-Anwendungsbereitstellung („WinAppDeployCmd.exe“) ist ein Befehlszeilentool, mit dem du eine App für die Universelle Windows-Plattform (UWP) von einem Windows 10-Computer auf jedem Windows 10-Gerät bereitstellen kannst.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d6c8383a5b0041d5edf6e0c2c8d94acf82572d13
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "70808449"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“

Die Windows-Anwendungsbereitstellung („WinAppDeployCmd.exe“) ist ein Befehlszeilentool, mit dem du eine App für die Universelle Windows-Plattform (UWP) von einem Windows 10-Computer auf jedem Windows 10-Gerät bereitstellen kannst. Dieses Tool ermöglicht die Bereitstellung eines App-Pakets, wenn das Windows 10-Gerät über USB verbunden oder im selben Subnetz verfügbar ist, ohne dass Microsoft Visual Studio oder die Projektmappe für diese App erforderlich sind. Sie können die App auch bereitstellen, ohne sie zuerst zu einem Remote-PC oder zu Xbox One zu verpacken. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden.

Um das Tool WinAppDeployCmd über eine Eingabeaufforderung oder Skriptdatei auszuführen, muss lediglich das Windows 10 SDK installiert sein. Wenn du eine App mit „WinAppDeployCmd.exe“ installierst, werden die APPX/MSIX-Datei oder „AppxManifest“ (für lose Dateien) verwendet, um die App auf ein Windows 10-Gerät querzuladen. Mit diesem Befehl wird nicht das für Ihre App erforderliche Zertifikat installiert. Zum Ausführen der App muss sich das Windows 10-Gerät im Entwicklermodus befinden oder bereits über das installierte Zertifikat verfügen.

Um eine Bereitstellung auf mobilen Geräten auszuführen, müssen Sie zunächst ein Paket erstellen. Weitere Informationen finden Sie [hier](/windows/msix/package/packaging-uwp-apps).

Das Tool **WinAppDeployCmd.exe** befindet sich auf deinem Windows 10-PC unter: **C:\\Programme (x86)\\Windows Kits\\10\\bin\\&lt;SDK-Version&gt;\\x86\\WinAppDeployCmd.exe** (basierend auf deinem Installationspfad für das SDK).

> [!NOTE]
> Ab Version 15063 des SDK wird das SDK nebeneinander in versionsspezifischen Ordnern installiert. Frühere SDKs (bis einschließlich 14393) werden direkt in den übergeordneten Ordner geschrieben.

Verbinde zunächst das Windows 10-Gerät mit demselben Subnetz oder direkt über eine USB-Verbindung mit deinem Windows 10-Computer. Verwenden Sie anschließend die folgende Syntax und die Beispiele zu diesem Befehl weiter unten in diesem Artikel, um die UWP-App bereitzustellen:

## <a name="winappdeploycmd-syntax-and-options"></a>Syntax und Optionen für WinAppDeployCmd

Dies ist die allgemeine Syntax für **WinAppDeployCmd.exe**:

```CMD
WinAppDeployCmd command -option <argument>
```

Hier findest du einige zusätzliche Syntaxbeispiele für die Verwendung verschiedener Befehle:

```CMD
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

Sie können eine App auf dem Zielgerät installieren oder deinstallieren oder eine bereits installierte App aktualisieren. Um die Daten oder Einstellungen beizubehalten, die von einer bereits installierten App gespeichert wurden, verwenden Sie die **update**-Optionen anstelle der **install**-Optionen.

Die folgende Tabelle enthält die Befehle für **WinAppDeployCmd.exe**.

| **Befehl**  | **Beschreibung**                                                     |
|--------------|---------------------------------------------------------------------|
| Geräte      | Zeigt die Liste verfügbarer Netzwerkgeräte an.                         |
| installieren      | Installiert ein UWP-App-Paket auf dem Zielgerät.                     |
| Update       | Aktualisiert eine UWP-App, die bereits auf dem Zielgerät installiert ist.    |
| list         | Zeigt die Liste der auf dem angegebenen Zielgerät installierten UWP-Apps an. |
| uninstall    | Deinstalliert das angegebene App-Paket vom Zielgerät.         |
| deployfiles  | Kopiert die App mit loser Datei am Zielpfad zum relativen Remotepfad auf dem Gerät.|
| registerfiles| Registriert die App mit loser Datei am Remotebereitstellungsverzeichnis.         |
| addcreds     | Fügt einer Xbox Anmeldeinformationen hinzu, um auf einen Netzwerkspeicherort für die App-Registrierung zuzugreifen.|
| getcreds     | Ruft Netzwerkanmeldeinformationen ab, die das Ziel beim Ausführen einer Anwendung von einer Netzwerkfreigabe verwendet.|
| deletecreds  | Löscht Netzwerkanmeldeinformationen, die das Ziel beim Ausführen einer Anwendung von einer Netzwerkfreigabe verwendet.|

Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

| **Befehl**  | **Beschreibung**  |
|--------------|------------------|
| -h (-help)       | Zeigt die Befehle, Optionen und Argumente an. |
| -ip              | Die IP-Adresse des Zielgeräts. |
| -g (-guid)       | Der eindeutige Bezeichner des Zielgeräts.|
| -d (-dependency) | (Optional) Gibt den Abhängigkeitspfad für jede Paketabhängigkeit an. Wenn kein Pfad angegeben ist, sucht das Tool Abhängigkeiten im Stammverzeichnis des App-Pakets und in den SDK-Verzeichnissen.|
| -f (-file)       | Der Dateipfad für das App-Paket, das installiert, aktualisiert oder deinstalliert werden soll.|
| -p (-package)    | Der vollständige Paketname für das zu deinstallierende App-Paket. (Mit dem list-Befehl kannst du den vollständigen Namen von Paketen suchen, die auf dem Gerät bereits installiert sind.) |
| -pin             | Eine PIN, falls zum Herstellen einer Verbindung mit dem Zielgerät erforderlich. (Wenn eine Authentifizierung erforderlich ist, wirst du aufgefordert, den Versuch mit der Option „-pin“ zu wiederholen.) |
| -credserver      | Der Servername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -credusername    | Der Benutzername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -credpassword    | Das Kennwort der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -connecttimeout  | Die Zeitüberschreiung in Sekunden, die bei der Herstellung der Verbindung mit dem Gerät verwendet wird. |
| -remotedeploydir | Relativer Verzeichnispfad/Name zum Kopieren von Dateien auf das Remotegerät. Dies ist ein bekannter, automatisch bestimmter Remotebereitstellungsordner. |
| -deleteextrafile | Wechselt, um anzugeben, ob vorhandene Dateien im Remoteverzeichnis gelöscht werden sollen, um mit dem Quellverzeichnis übereinzustimmen. |

Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

| **Argument**           | **Beschreibung**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Zeitüberschreitung in Sekunden. (Der Standardwert ist 10.)                                          |
| &lt;address&gt;        | Die IP-Adresse oder der eindeutige Bezeichner des Zielgeräts.                        |
| &lt;a&gt;&lt;b&gt; ... | Der Abhängigkeitspfad für die einzelnen App-Paket-Abhängigkeiten.                    |
| &lt;p&gt;              | Eine alphanumerische PIN, die in den Geräteeinstellungen angezeigt und zum Herstellen einer Verbindung verwendet wird. |
| &lt;path&gt;           | Dateisystempfad.                                                            |
| &lt;name&gt;           | Vollständiger Paketname für das zu deinstallierende App-Paket.                          |
| &lt;server&gt;         | Server im Dateinetzwerk.                                                  |
| &lt;username&gt;       | Benutzer für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk.      |
| &lt;password&gt;       | Kennwort für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk. |
| &lt;remotedeploydir&gt;| Verzeichnis auf dem Gerät relativ zum Bereitstellungsspeicherort.                      |

## <a name="winappdeploycmdexe-examples"></a>Beispiele für „WinAppDeployCmd.exe“

Nachfolgend findest du einige Beispiele für die Bereitstellung über die Befehlszeile mithilfe der Syntax von **WinAppDeployCmd.exe**.

Zeigt die für die Bereitstellung verfügbaren Geräte an. Der Befehl verursacht in drei Sekunden ein Timeout.

``` CMD
WinAppDeployCmd devices 3
```

Installiert die App aus dem Paket „MyApp.appx“, das sich im Downloadverzeichnis deines PC befindet, mit der PIN A1B2C3 auf ein Windows 10-Gerät mit der IP-Adresse 192.168.0.1, um eine Verbindung mit dem Gerät herzustellen.

``` CMD
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Deinstalliert das angegebene Paket (unter Verwendung des vollständigen Namens) von einem Windows 10-Gerät mit der IP-Adresse 192.168.0.1. Mit dem list-Befehl können Sie die vollständigen Namen aller Pakete anzeigen, die auf einem Gerät installiert sind.

``` CMD
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Aktualisiert die bereits auf dem Windows 10-Gerät mit der IP-Adresse 192.168.0.1 installierte App unter Verwendung des angegebenen App-Pakets.

``` CMD
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Stellt die Dateien einer App auf einem PC oder auf Xbox mit der IP-Adresse 192.168.0.1 im gleichen Ordner wie das AppxManifest im Verzeichnis app1_F5 am Bereitstellungspfad des Geräts bereit.

``` CMD
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registriert die App im Verzeichnis app1_F5 am Bereitstellungspfad des PCs oder der Xbox mit 192.168.0.1.

``` CMD
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Verwenden von WinAppDeployCmd für die Einrichtung von über einen PC ausgeführten Bereitstellungen auf Xbox One

Das Ausführen über einen PC ermöglicht Ihnen die Bereitstellung einer UWP-Anwendung auf einer Xbox One, ohne die Binärdateien zu kopieren. Die Binärdateien werden stattdessen in einer Netzwerkfreigabe im selben Netzwerk wie die Xbox gehostet.  Dazu benötigen Sie eine für Entwickler entsperrte Xbox One und eine UWP-Anwendung mit loser Datei in einem Netzlaufwerk, das auf die Xbox zugreifen kann.

Führen Sie Folgendes aus, um die App zu registrieren:

``` CMD
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
