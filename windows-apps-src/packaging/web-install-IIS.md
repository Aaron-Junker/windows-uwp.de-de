---
title: Installieren einer UWP-App von einem IIS-Server
description: Dieses Tutorial veranschaulicht, wie Sie einen IIS-Server einrichten, stellen Sie sicher, dass Ihre Web-app kann app-Paketen zu hosten und aufrufen und effektive Verwendung der App-Installer.
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Uwp, app-Installer, AppInstaller, per sideload übertragen, im Zusammenhang festgelegt ist, optionale Pakete, IIS-Server
ms.localizationpriority: medium
ms.openlocfilehash: 6a4512229a29a7adc59d6b61edd596eaeb56a5a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623565"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Installieren einer UWP-App von einem IIS-Server

Dieses Tutorial veranschaulicht, wie Sie einen IIS-Server einrichten, stellen Sie sicher, dass Ihre Web-app kann app-Paketen zu hosten und aufrufen und effektive Verwendung der App-Installer.

Mit der App-Installer-App können Entwickler und IT-Spezialisten Windows 10-Apps verteilen, indem sie diese in ihrem eigenen Content Delivery Network (CDN) hosten. Das ist nützlich für Unternehmen, die ihre Apps nicht im Microsoft Store veröffentlichen möchten oder müssen, aber weiterhin die Windows 10-Verpackungs- und -Bereitstellungsplattform nutzen möchten. 

## <a name="setup"></a>Setup

Um erfolgreich mit diesem Tutorial durchlaufen, benötigen Sie Folgendes:

1. Visual Studio 2017  
2. Webentwicklungstools und IIS 
3. UWP-App-Paket – das App-Paket, die Sie verteilen möchten

Optional: [Startprojekt](https://github.com/AppInstaller/MySampleWebApp) auf GitHub. Dies ist hilfreich, wenn Sie keine app-Pakete mit arbeiten, aber Sie dennoch erfahren, wie Sie dieses Feature zu verwenden möchten.

## <a name="step-1---install-iis-and-aspnet"></a>Schritt 1: Installieren von IIS und ASP.NET 

[Internet Information Services](https://www.iis.net/) ist ein Windows-Feature, das über das Menü "Start" installiert werden kann. In **Menü "Start"** suchen Sie nach **Aktivieren von Windows-Funktionen ein- oder ausschalten**.

Suchen und auswählen **Internet Information Services** zum Installieren von IIS.

> [!NOTE]
> Sie müssen nicht alle Kontrollkästchen unter Internet Information Services auswählen. Nur die Ereignisse ausgewählt, wenn Sie überprüfen, **Internet Information Services** sind ausreichend.

Sie müssen auch ASP.NET 4.5 oder höher installieren. Um es zu installieren, suchen **Internetinformationsdienste -> World Wide Web Services -> Application Development Features**. Wählen Sie eine Version von ASP.NET an, die größer als oder gleich auf ASP.NET 4.5 ist.

![Installieren von ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Schritt 2: Installieren von Visual Studio 2017 und Web-Entwicklungstools 

[Installieren Sie Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) , wenn Sie nicht bereits installiert haben. Wenn Sie bereits Visual Studio 2017 verfügen, stellen Sie sicher, dass die folgenden Workloads installiert sind. Wenn die arbeitsauslastungen nicht auf Ihre Installation vorhanden sind, führen Sie zusammen mit Visual Studio-Installer (über das Startmenü gefunden).  

Wählen Sie während der Installation **ASP.NET und Webentwicklung** und alle anderen arbeitsauslastungen, die Sie interessiert sind. 

Sobald die Installation abgeschlossen ist, starten Sie Visual Studio, und Erstellen eines neuen Projekts (**Datei** -> **neues Projekt**).

## <a name="step-3---build-a-web-app"></a>Schritt 3: Erstellen einer WebApp

Starten Sie Visual Studio 2017 als **Administrator** und erstellen Sie ein neues **Visual C# Webanwendung** Projekt mit einer **leere** Projektvorlage. 

![Neues Projekt](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Schritt 4: Konfigurieren von IIS mit Ihrer Web-App 

Klicken Sie mit der rechten Maustaste auf das Root-Projekt im Projektmappen-Explorer, und wählen **Eigenschaften**.

Wählen Sie in den Web-app-Eigenschaften, die **Web** Registerkarte. In der **Server** Abschnitt **lokale IIS** aus der Dropdown-Menü, und klicken Sie auf **virtuelles Verzeichnis erstellen**. 

![Registerkarte "Web"](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Schritt 5: Hinzufügen eines app-Pakets mit einer Webanwendung 

Fügen Sie das app-Paket, das Sie bei der Webanwendung verteilen möchte. Können Sie das app-Paket, die Teil des bereitgestellten [Starter Projektpakete](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) auf GitHub, wenn Sie ein app-Paket zur Verfügung haben. Die Zertifikat (MySampleApp.cer), mit dem das Paket signiert wurde, ist ebenfalls im Beispiel auf GitHub enthalten. Sie müssen das Zertifikat auf Ihrem Gerät vor der Installation der app (Schritt 9) installiert haben.

In der Webanwendung von Starter-Projekt ein neuer Ordner die Web-app Namens hinzugefügt wurde `packages` , enthält die app-Pakete verteilt werden. Um den Ordner in Visual Studio zu erstellen, mit der rechten Maustaste auf den Stamm des Projektmappen-Explorer wählen **hinzufügen** -> **neuer Ordner** und nennen Sie sie `packages`. Um app-Pakete in den Ordner hinzuzufügen, klicken Sie mit der rechten Maustaste auf die `packages` Ordner, und wählen **hinzufügen** -> **vorhandenes Element...**  , und navigieren Sie zum Speicherort app-Paket. 

![Paket hinzufügen](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Schritt 6: Erstellen einer Webseite

Diese Beispiel-Web-app verwendet einfachen HTML-Code. Sie können Ihre Web-app nach Bedarf Ihre Anforderungen zu erstellen. 

Klicken Sie mit der rechten Maustaste auf das Stammprojekt des Projektmappen-Explorer, wählen **hinzufügen** -> **neues Element**, und fügen Sie einen neuen **HTML-Seite** aus der **Web**Abschnitt.

Sobald die HTML-Seite erstellt wurde, klicken Sie mit der rechten Maustaste auf die HTML-Seite im Projektmappen-Explorer, und wählen Sie **als Startseite festlegen**.  

Doppelklicken Sie auf die HTML-Datei, um sie im Code-Editor-Fenster zu öffnen. In diesem Tutorial werden nur die Elemente im erforderlichen auf der Webseite zum Aufrufen der App-Installer-app erfolgreich zum Installieren einer Windows 10-app verwendet werden. 

Der folgenden HTML-Code in Ihre Webseite einschließen. Die Taste, um die App-Installer erfolgreich aufrufen ist die Verwendung der benutzerdefinierte Schema, das App-Installer mit dem Betriebssystem registriert: `ms-appinstaller:?source=`. Finden Sie im Codebeispiel unten Weitere Informationen.

> [!NOTE]
> Stellen Sie sicher, dass die URL-Pfad angegeben, nach das benutzerdefinierte Schema der Projekt-Url auf der Registerkarte "Web" Ihrer Visual Studio-Lösung entspricht.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Schritt 7: Konfigurieren der Web-app für app-Paket-MIME-Typen

Öffnen der **"Web.config"** Datei im Projektmappen-Explorer, und fügen Sie die folgenden Zeilen in der `<configuration>` Element. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Schritt 8: Hinzufügen der Loopback-Ausnahme für App-Installer

Aufgrund der Netzwerkisolation sind UWP-apps wie App-Installer verwenden IP-Loopback-Adressen wie beschränkt http://localhost/. Bei lokalen IIS-Server zu verwenden, muss der Loopback-Ausnahme-Liste App-Installer hinzugefügt werden. 

Zu diesem Zweck öffnen Sie **Eingabeaufforderung** als ein **Administrator** , und geben Sie die folgenden: ''' Befehlszeile CheckNetIsolation.exe LoopbackExempt - ein-n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Sie werden feststellen `microsoft.desktopappinstaller_8wekyb3d8bbwe` in der Liste.

Sobald die lokale Überprüfung von app-Installation über einen App-Installer abgeschlossen ist, können Sie die Loopback-Ausnahme entfernen, die Sie in diesem Schritt von hinzugefügt:

```Command Line CheckNetIsolation.exe LoopbackExempt -d -n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
