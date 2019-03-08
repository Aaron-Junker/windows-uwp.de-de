---
title: Erstellen einer App-Installer-Datei mit Visual Studio
description: Hier erfahren Sie, wie Sie Visual Studio verwenden, um automatische Updates mithilfe der .appinstaller-Datei zu aktivieren.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, app-installer, AppInstaller, querladen
ms.localizationpriority: medium
ms.openlocfilehash: 3aa7622fe408fcbc1f8da4c0fe0c6b8d54dd2cd6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648485"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>Erstellen einer App-Installer-Datei mit Visual Studio

Ab Windows 10, Version 1803 und Visual Studio 2017 Update 15.7, mittels sideload übertragenen apps können so konfiguriert werden Empfang automatischer Updates, die mit einem `.appinstaller` Datei. Visual Studio unterstützt das Aktivieren dieser Updates.

## <a name="app-installer-file-location"></a>Speicherort der App-Installer-Datei
Die `.appinstaller`-Datei in kann an einem freigegebenen Speicherort wie einem HTTP-Endpunkt oder einem freigegebenen UNC-Ordner gehostet werden, und enthält den Pfad zum Suchen der zu installierenden App-Pakete. Benutzer installieren die App vom freigegebenen Speicherort und aktivieren regelmäßige Überprüfungen auf neue Updates. 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>Konfigurieren des Projekts für die richtige Windows-Version

Sie können die `TargetPlatformMinVersion`-Eigenschaft entweder beim Erstellen des Projekts konfigurieren, oder später in den Projekteigenschaften ändern. 

>[!IMPORTANT]
> Die app-Installer-Datei wird nur generiert, wenn die `TargetPlatformMinVersion` ist Windows 10, Version 1803 oder höher.


### <a name="create-packages"></a>Erstellen von Paketen

Zum Verteilen einer Apps per sideload müssen Sie eine app-Paket (.appx/.msix) oder einen app-Bündel (.appxbundle/.msixbundle) erstellen und veröffentlichen es in einem freigegebenen Speicherort.

Verwenden Sie dazu den Assistenten **App-Pakete erstellen** in Visual Studio mit den folgenden Schritten.

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Store** -> **App-Pakete erstellen** aus.  

![Kontextmenü mit Navigation zu „App-Pakete erstellen“](images/packaging-screen2.jpg)   

Der Assistent **App-Pakete erstellen** wird angezeigt.

2. Wählen Sie **Ich möchte Pakete zum Querladen erstellen.** und **Automatische Updates aktivieren** aus.  

![Dialogfeld „Ihre Pakete erstellen“](images/select-sideloading.png)  

Die Option **Automatische Updates aktivieren** ist nur aktiviert, wenn die `TargetPlatformMinVersion` des Projekts auf die richtige Version von Windows 10 festgelegt ist.

3. Im Dialogfeld **Auswählen und Konfigurieren von Paketen** können Sie die unterstützten Architekturkonfigurationen auswählen. Wenn Sie ein Bündel auswählen, wird ein einzelnes Installationsprogramm generiert. Wenn Sie jedoch kein Bündel wünschen und ein Paket pro Architektur bevorzugen, erhalten Sie auch eine Installationsdatei pro Architektur.  Wenn Sie nicht sicher sind, welche Architektur(en) Sie auswählen sollen, oder wenn Sie mehr darüber erfahren möchten, welche Architekturen von verschiedenen Geräten verwendet werden, finden Sie weitere Informationen unter [App-Paketarchitekturen](device-architecture.md).

4. Konfigurieren Sie zusätzliche Details wie die Versionsnummer oder den Ausgabespeicherort des Pakets.

![Dialogfeld „App-Pakete erstellen“ mit Paketkonfiguration](images/packaging-screen5.jpg)  

5. Wenn Sie in Schritt 2 **Automatische Updates aktivieren** aktiviert haben, wird das Dialogfeld **Konfigurieren der Update-Einstellungen** angezeigt. Hier können Sie die **Installations-URL** und die Häufigkeit der Update-Prüfungen angeben.

![Anzeige des Dialogfelds „Konfigurieren der Update-Einstellungen” mit Konfiguration des Veröffentlichungsortes](images/sideloading-screen.png)  

6. Wenn Ihre App erfolgreich verpackt wurde, wird in einem Dialogfeld der Speicherort des Ausgabeordners, der Ihr App-Paket enthält, angezeigt. Der Ausgabeordner enthält alle Dateien, die für das Querladen der App erforderlich sind, einschließlich einer HTML-Seite, die zum Bewerben Ihrer App verwendet werden kann.

### <a name="publish-packages"></a>Veröffentlichung von Paketen

Um die Anwendung verfügbar zu machen, müssen die generierten Dateien am angegebenen Speicherort veröffentlicht werden:

#### <a name="publish-to-shared-folders-unc"></a>Veröffentlichung in freigegebenen Ordnern (UNC)

Wenn Sie Ihre Pakete über freigegebene UNC (Universal Naming Convention)-Ordner veröffentlichen möchten, konfigurieren Sie den Ausgabeordner des App-Pakets sowie die Installations-URL (Details finden Sie unter Schritt 6) im selben Pfad. Der Assistent generiert die Dateien am richtigen Speicherort, und Benutzer erhalten sowohl die App, als auch die zukünftigen Updates vom selben Pfad.

#### <a name="publish-to-a-web-location-http"></a>Veröffentlichung an einem Webspeicherort (HTTP)

Für die Veröffentlichung an einem Webspeicherort ist ein Zugriff erforderlich, damit Inhalte auf dem Webserver veröffentlicht werden können. Dabei muss sichergestellt werden, dass die endgültige URL der im Assistenten definierten Installations-URL entspricht (Details finden Sie unter Schritt 6). In der Regel werden das File Transfer Protocol (FTP) oder das SSH-File Transfer Protocol (SFTP) zum Hochladen der Dateien verwendet, es gibt aber je nach Ihrem Web-Provider auch andere Veröffentlichungsmethoden wie MSDeploy, SSH oder Blob Storage.

Um den Webserver zu konfigurieren, müssen Sie die MIME-Typen überprüfen, die für die verwendeten Dateitypen verwendet werden. Dies ist ein Beispiel aus `web.config` für Internetinformationsdienste (IIS):

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















