---
title: Installation einer UWP-App von einem Azure-Webserver
description: In diesem Lernprogramm wird gezeigt, wie Sie einen Azure-Webserver einrichten, überprüfen, ob Ihre Web-App kann App-Pakete hosten kann, und App-Installer auf effektive Weise aufrufen und verwenden.
ms.date: 11/30/2018
ms.topic: article
keywords: Windows 10, UWP, App-Installer, AppInstaller, querladen, zusammengehörig, optionale Pakete, Azure-Webserver
ms.localizationpriority: medium
ms.openlocfilehash: 074a8e9941d4314bb35c28b0ee296e9d86fa23a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608795"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>Installieren einer UWP-App aus einer Azure-Web-App

Mit der App-Installer-App können Entwickler und IT-Spezialisten Windows 10-Apps verteilen, indem sie diese in ihrem eigenen Content Delivery Network (CDN) hosten. Das ist nützlich für Unternehmen, die ihre Apps nicht im Microsoft Store veröffentlichen möchten oder müssen, aber weiterhin die Windows 10-Verpackungs- und -Bereitstellungsplattform nutzen möchten.

In diesem Thema sind die Schritte zum Konfigurieren eines Azure-Webservers zum Hosten von UWP-App-Paketen aufgeführt, und Sie erfahren, wie Sie die App-Installer-App verwenden, um die App-Pakete zu installieren.

## <a name="setup"></a>Setup

Für eine erfolgreiche Durchführung dieses Lernprogramms benötigen Sie Folgendes:
 
1. Microsoft Azure-Abonnement 
2. UWP-App-Paket – das App-Paket, die Sie verteilen möchten

Optional: [Startprojekt](https://github.com/AppInstaller/MySampleWebApp) auf GitHub. Dies ist hilfreich, wenn Sie kein App-Paket oder keine Webseite zum Arbeiten verfügbar haben, aber dennoch lernen möchten, wie Sie dieses Feature verwenden.

### <a name="step-1---get-an-azure-subscription"></a>Schritt 1: Erwerben eines Azure-Abonnements
Informationen zum Erwerben eines Azure-Abonnements finden Sie auf der [Azure Kontoseite](https://azure.microsoft.com/free/). Für die Zwecke dieses Lernprogramms können Sie eine kostenlose Mitgliedschaft verwenden.

### <a name="step-2---create-an-azure-web-app"></a>Schritt 2: Erstellen einer Azure-Web-App 
Klicken Sie auf der Azure-Portalseite auf die Schaltfläche **+ Ressource erstellen**, und wählen Sie dann **Web-App** aus.

![erstellen](images/azure-create-app.png)

Erstellen Sie einen eindeutigen **App-Namen**, und behalten Sie für die restlichen Felder die Standardwerte bei. Klicken Sie auf **Erstellen**, um den Assistenten zum Erstellen von Web-Apps abzuschließen. 

![Erstellen einer Web-App](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>Schritt 3: Hosten des App-Pakets und der Webseite 
Nach die Web-App erstellt wurde, können Sie über das Dashboard im Azure-Portal darauf zugreifen. In diesem Schritt werden wir eine einfache Webseite mit der GUI des Azure-Portals erstellen.

Nach Auswahl der neu erstellten Web-App über das Dashboard, verwenden Sie das Suchfeld, um nach dem **App-Dienst-Editor** zu suchen und ihn zu öffnen. 

Im Editor gibt es eine standardmäßige `hostingstart.html`-Datei. Klicken Sie mit der rechten Maustaste in den leeren Bereich des Datei-Explorer-Panels, und wählen Sie **Dateien hochladen** aus, um das Hochladen Ihrer App-Pakete zu beginnen.

> [!NOTE]
> Sie können das App-Paket verwenden, das Teil des bereitgestellten [Startprojekt](https://github.com/AppInstaller/MySampleWebApp)-Repositorys auf GitHub ist, falls Ihnen kein App-Paket zur Verfügung steht. Die Zertifikat (MySampleApp.cer), mit dem das Paket signiert wurde, ist ebenfalls im Beispiel auf GitHub enthalten. Sie müssen das Zertifikat vor der Installation der App auf Ihrem Gerät installieren.

![Upload](images/azure-upload-file.png)

Klicken Sie mit der rechten Maustaste in den leeren Bereich des Datei-Explorer-Panels, und wählen Sie **Neue Dateien** aus, um eine neue Datei zu erstellen. Geben Sie der Datei den Namen: `default.html`.

Wenn Sie das im [Startprojekt](https://github.com/AppInstaller/MySampleWebApp) bereitgestellte App-Paket verwenden, kopieren Sie den folgenden HTML-Code in die neu erstellte Webseite `default.html`. Wenn Sie ein eigenes App-Paket verwenden, ändern Sie die App-Dienst-URL (die URL nach `source=`). Sie können die App-Dienst-URL von der Übersichtsseite Ihrer App im Azure-Portal abrufen.

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>Schritt 4: Konfigurieren der Web-App für App-Paket-MIME-Typen

Fügen Sie der Web-App eine neue Datei mit dem folgenden Namen hinzu: `Web.config`. Öffnen Sie die Datei `Web.config` im Explorer, und fügen Sie die folgenden Zeilen hinzu. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
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
</configuration>
```

### <a name="step-5---run-and-test"></a>Schritt 5: Ausführen und Testen

Um die von Ihnen erstellte Webseite zu starten, geben die URL aus Schritt 3 in den Browser ein, gefolgt von `/default.html`. 

![Edge](images/edge.png)

Klicken Sie auf „Meine Beispiel-App installieren“, um App-Installer zu starten und Ihr App-Paket zu installieren. 

## <a name="troubleshooting-issues"></a>Problembehandlung

### <a name="app-installer-app-fails-to-install"></a>Fehler bei der Installation der App-Installer-App 
Die App-Installation schlägt fehl, wenn das Zertifikat, mit dem das App-Paket signiert ist, nicht auf dem Gerät installiert ist. Um dieses Problem zu beheben, müssen Sie das Zertifikat vor der Installation der App installieren. Wenn Sie ein App-Paket für die öffentliche Verteilung hosten, wird empfohlen, das App-Paket mit einem Zertifikat von einer Zertifizierungsstelle zu signieren. 

![App-Zertifikat](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>Beim Klicken auf den Link geschieht nichts 
Stellen Sie sicher, dass die App-Installer-App installiert ist. Wechseln Sie zu **Einstellungen** -> **Apps und Features**, und suchen Sie in der Liste der installierten Apps nach App-Installer. 

