---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“
description: Bereitstellung von Windows-Anwendung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit denen eine universelle Windows-Plattform (UWP)-app aus einem Windows 10-PC zu Windows 10-Geräten bereitstellen.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 06107691a4551ae2af05e63c1db810485273dc9b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372815"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“


Bereitstellung von Windows-Anwendung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit denen eine universelle Windows-Plattform (UWP)-app aus einem Windows 10-PC zu Windows 10-Geräten bereitstellen. Sie können dieses Tool verwenden, um ein app-Paket bereitzustellen, wenn das Windows 10-Gerät über USB verbunden bzw. das sich im gleichen Subnetz verfügbar ist, ohne Microsoft Visual Studio oder die Lösung für die app. Sie können die App auch bereitstellen, ohne sie zuerst zu einem Remote-PC oder zu Xbox One zu verpacken. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden.

Sie benötigen nur das Windows 10-SDK installiert wird, führen Sie das WinAppDeployCmd-Tool über eine Eingabeaufforderung oder eine Skriptdatei an. Wenn Sie eine app mit WinAppDeployCmd.exe installieren, verwendet diese die.appx/.msix-Datei oder das AppxManifest (für lose Dateien) zu Ihrer app auf einem Windows 10-Gerät querladen. Mit diesem Befehl wird nicht das für Ihre App erforderliche Zertifikat installiert. Führen Sie die app muss das Windows 10-Gerät im Entwicklermodus oder bereits über das installierte Zertifikat verfügen.

Um eine Bereitstellung auf mobilen Geräten auszuführen, müssen Sie zunächst ein Paket erstellen. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Die **WinAppDeployCmd.exe** Tool befindet sich auf Ihrem Windows 10-PC: **"C:"\\Programmdateien (x86)\\Windows-Kits\\10\\Bin\\&lt;SDK-Version&gt;\\X86\\WinAppDeployCmd.exe** () basierend auf dem Installationspfad für das SDK). 
> [!NOTE]
> In Version 15063 und höher des SDK ist das SDK nebeneinander in versionsspezifischen Ordnern installiert.  Frühere SDKs (vor und einschließlich 14393) werden direkt in den übergeordneten Ordner geschrieben.

Verbinden Sie Ihres Windows 10-Geräts mit dem gleichen Subnetz zum ersten Mal, oder direkt mit Ihrem Windows 10-Computer mit einem USB-Verbindung verbinden. Verwenden Sie anschließend die folgende Syntax und die Beispiele zu diesem Befehl weiter unten in diesem Artikel, um die UWP-App bereitzustellen:

## <a name="winappdeploycmd-syntax-and-options"></a>Syntax und Optionen für WinAppDeployCmd

Dies ist die allgemeine, für **WinAppDeployCmd.exe** verwendete Syntax:
```syntax
WinAppDeployCmd command -option <argument>
```

Hier finden Sie einige zusätzliche Syntaxbeispiele für die Verwendung verschiedener Befehle:
```syntax
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
| -p (-package)    | Der vollständige Paketname für das zu deinstallierende App-Paket. (Mit dem „list”-Befehl können Sie den vollständigen Namen von Paketen suchen, die bereits auf dem Gerät installiert sind) |
| -pin             | Eine PIN, falls zum Herstellen einer Verbindung mit dem Zielgerät erforderlich. (Wenn eine Authentifizierung erforderlich ist, werden Sie aufgefordert, den Versuch mit der Option „-pin” zu wiederholen) |
| -credserver      | Der Servername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -credusername    | Der Benutzername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -credpassword    | Das Kennwort der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel. |
| -connecttimeout  | Die Zeitüberschreiung in Sekunden, die bei der Herstellung der Verbindung mit dem Gerät verwendet wird. |
| -remotedeploydir | Relativer Verzeichnispfad/Name zum Kopieren von Dateien auf das Remote-Gerät. Dies ist ein bekannter, automatisch bestimmter Remotebereitstellungsordner. |
| -deleteextrafile | Wechselt, um anzugeben, ob vorhandene Dateien im Remoteverzeichnis gelöscht werden sollen, um mit dem Quellverzeichnis übereinzustimmen. |


Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

| **Argument**           | **Beschreibung**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Zeitüberschreitung in Sekunden. (Der Standardwert ist 10.)                                          |
| &lt;address&gt;        | Die IP-Adresse oder der eindeutige Bezeichner des Zielgeräts.                        |
| &lt;a&gt;&lt;b&gt; ... | Der Abhängigkeitspfad für die einzelnen App-Paket-Abhängigkeiten.                    |
| &lt;p&gt;              | Eine alphanumerische PIN, die in den Geräteeinstellungen angezeigt und zum Herstellen einer Verbindung verwendet wird. |
| &lt;path&gt;           | Dateisystempfad.                                                            |
| &lt;name&gt;           | Vollständiger Paketname für das zu deinstallierende App-Paket.                          |
| &lt;server&gt;         | Server im Dateinetzwerk.                                                  |
| &lt;username&gt;       | Benutzer für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk.      |
| &lt;password&gt;       | Kennwort für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk. |
| &lt;remotedeploydir&gt;| Verzeichnis auf dem Gerät relativ zum Bereitstellungsspeicherort.                      |

 
## <a name="winappdeploycmdexe-examples"></a>Beispiele für „WinAppDeployCmd.exe“

Im Folgenden finden Sie einige Beispiele dazu, wie Sie mithilfe der Syntax für **WinAppDeployCmd.exe** eine Bereitstellung über die Befehlszeile ausführen.

Zeigt die für die Bereitstellung verfügbaren Geräte an. Der Befehl verursacht in drei Sekunden ein Timeout.

``` syntax
WinAppDeployCmd devices 3
```

Installiert die app aus dem Paket MyApp.appx, die in Ihrer PC Downloads-Verzeichnis auf einem Windows 10-Gerät mit einer IP-Adresse 192.168.0.1, klicken Sie mit einer PIN A1B2C3 zum Herstellen einer Verbindung mit dem Gerät ist.

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Deinstalliert das angegebene Paket (unter Verwendung des vollständigen Namens) von einem Windows 10-Gerät mit der IP-Adresse 192.168.0.1. Mit dem list-Befehl können Sie die vollständigen Namen aller Pakete anzeigen, die auf einem Gerät installiert sind.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Aktualisiert die app, die bereits auf dem Windows 10-Gerät mit einer IP-Adresse 192.168.0.1, die mit dem angegebenen app-Paket installiert ist.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Stellt die Dateien einer App auf einem PC oder auf Xbox mit der IP-Adresse 192.168.0.1 im gleichen Ordner wie das AppxManifest im Verzeichnis app1_F5 am Bereitstellungspfad des Geräts bereit.

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registriert die App im Verzeichnis app1_F5 am Bereitstellungspfad des PCs oder der Xbox mit 192.168.0.1.

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Verwenden von WinAppDeployCmd für die Einrichtung von über einen PC ausgeführten Bereitstellungen auf Xbox One

Das Ausführen über einen PC ermöglicht Ihnen die Bereitstellung einer UWP-Anwendung auf einer Xbox One, ohne die Binärdateien zu kopieren. Die Binärdateien werden stattdessen in einer Netzwerkfreigabe im selben Netzwerk wie die Xbox gehostet.  Dazu benötigen Sie eine für Entwickler entsperrte Xbox One und eine UWP-Anwendung mit loser Datei in einem Netzlaufwerk, das auf die Xbox zugreifen kann.

Führen Sie Folgendes aus, um die App zu registrieren:
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
