---
title: Installieren von UWP-Apps von einer Webseite
description: In diesem Abschnitt werden die erforderlichen Schritte aufgeführt, damit Sie Benutzern die Installation Ihre Apps direkt von der Webseite ermöglichen können.
ms.date: 11/16/2017
ms.topic: article
keywords: Windows 10, UWP, App-Installer, AppInstaller, querladen, zugehörige Gruppe, optionale Pakete
ms.localizationpriority: medium
ms.openlocfilehash: 515beebd55049ecb4d0c6747fa7d37e76577ef7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623555"
---
# <a name="installing-uwp-apps-from-a-web-page"></a>Installieren von UWP-Apps von einer Webseite

Eine App muss in der Regel lokal auf einem Gerät verfügbar sein, bevor sie mit dem App-Installer installiert werden kann. Für das Web-Szenario bedeutet dies, dass der Benutzer das App-Paket aus dem Webserver herunterladen muss, nachdem es mit App-Installer installiert werden kann. Dies ist ineffizient und verschwendet Festplattenspeicher. Daher verfügt der App-Installer jetzt über integrierte Funktionen, um den Prozess zu optimieren.

Der App-Installer kann eine App direkt von einem Webserver installieren. Klickt der Benutzer auf einen gehosteten Weblink für App-Paket, wird der App-Installer automatisch aufgerufen. Der Benutzer wird dann auf die Seite der App-Informationen im App-Installer geleitet und ist dann einen Klick davon entfernt, direkt mit der App zu interagieren. 

Die direkte App-Installation ist nur im Windows 10 Fall Creators Update verfügbar und höher. Frühere Versionen von Windows (bis auf das Windows 10 Anniversary Update) unterstützen die [Web-Installation unter älteren Versionen von Windows 10](#web-install-experience). Diese Erfahrung ist nicht so reibungslos wie die direkte App-Installation, sie bietet allerdings erhebliche Verbesserungen gegenüber dem vorhandenen App-Installationsverfahren.
  
> [!NOTE]
> Die App-Installer-Version muss höher als 1.0.12271.0 sein, um dieses Features zu unterstützen.

## <a name="protocol-activation-scheme"></a>Protokollaktivierungsschema
Bei diesem Mechanismus wird der App-Installer mit dem Betriebssystem für ein Protokollaktivierungsschema registriert. Wenn Benutzer auf einen Weblink klicken, sucht der Browser mithilfe des Betriebssystems nach Apps, die für diesen Weblink registriert sind. Wenn das Schema mit dem vom App-Installer angegebenen Aktivierungsprotokollschema übereinstimmt, wird der App-Installer aufgerufen. Es ist wichtig zu beachten, dass dieser Mechanismus vom Browser unabhängig ist. Dies ist z. B. für Websiteadministratoren besonders nützlich, die beim Einbinden auf eine Webseite keine Unterschiede bei Webbrowsern berücksichtigen müssen. 

### <a name="requirements-for-protocol-activation-scheme"></a>Anforderungen für das Aktivierungsprotokollschema

1. Webserver benötigen Unterstützung für Anforderungen für Bytebereiche (HTTP/1.1)
    - Server, die HTTP/1.1-Protokoll unterstützen sollten Unterstützung für Anforderungen für Bytebereiche verfügen. 
2. Webserver müssen über die Windows 10-app-Paket-Inhaltstypen wissen
    - So deklarieren Sie die neuen Inhaltstypen als Teil des [Webkonfigurationsdatei](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types)

### <a name="how-to-enable-this-on-a-webpage"></a>So aktiviere ich dies auf einer Webseite 
App-Entwickler, die App-Pakete auf Ihren Websites hosten möchten, müssen folgenden Schritt durchführen:

Verleihen Sie Ihren App-Paket-URIs einen Präfix mit dem Aktivierungsschema `'ms-appinstaller:?source='`, damit der App-Installer registriert ist, wenn Sie auf Ihrer Webseite darauf verweisen. Siehe Beispiel für **MyApp-Webseite** für weitere Details. 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## <a name="signing-the-app-package"></a>Signieren des App-Pakets
Damit Benutzer Ihre App installieren, müssen Sie das App-Paket mit einem vertrauenswürdigen Zertifikat signieren. Sie können ein kostenpflichtiges Drittanbieterzertifikat einer vertrauenswürdigen Zertifizierungsstelle verwenden, um Ihr App-Paket zu signieren. Wenn ein Zertifikat von Drittanbietern verwendet wird, muss sich das Gerät des Benutzers im Querlade- oder Entwicklermodus befinden, um Ihre App zu installieren und auszuführen.

Wenn Sie eine App für Mitarbeiter in einem Unternehmen bereitstellen, können Sie ein von einem Unternehmen ausgestelltes Zertifikat zum Signieren der App verwenden. Beachten Sie, dass das Unternehmenszertifikat auf allen Geräten bereitgestellt werden muss, auf denen die App installiert wird. Weitere Informationen zum Bereitstellen von Unternehmens-Apps finden Sie unter [Enterprise-App-Verwaltung](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

## Webinstallationserfahrung auf früheren Versionen von Windows 10<a name="web-install-experience"></a>

Das Aufrufen des App-Installers über den Browser wird für alle Versionen von Windows 10 unterstützt, auf denen der App-Installer verfügbar ist (beginnend mit dem Anniversary Update). Die Funktion der direkten Installation von der Webseite, ohne dass das Paket zuerst heruntergeladen wurde, ist jedoch zunächst nur im Windows 10 Fall Creators Update verfügbar.  

Benutzer früherer Versionen von Windows 10 (mit App-Installer) können auch die Webinstallation von UWP-Apps über den App-Installer mit einer anderen Benutzererfahrung nutzen. Wenn diese Anwender auf den Weblink klicken, zeigt der App-Installer die Meldung **Download** des Pakets statt **Installieren** an. Nach dem Download startet der App-Installer das heruntergeladene Paket automatisch. Da das App-Paket aus dem Web heruntergeladen wird, werden diese Dateien über Microsoft SmartScreen einer Sicherheitsprüfung unterzogen. Nachdem der Benutzer die Zustimmung zum Fortfahren gibt und auf **Installieren** klickt, ist die App einsatzbereit.

Obwohl dieser Ablauf nicht ganz so nahtlos wie die direkte Installation auf dem Windows 10 Fall Creators Update ist, können Benutzer schnell mit der App interagieren. Mit diesem Ablauf muss sich der Benutzer keine Sorgen machen, dass die App-Paket-Dateien unnötigen Speicherplatz auf den Laufwerken einnimmt. Der App-Installer verwaltet den Speicherplatz effizient, indem er das Paket in den Ordner der App-Daten lädt und die Pakete löscht, wenn diese nicht mehr benötigt werden. 

Hier ist ein schneller Vergleich zwischen der Version des Windows 10 Fall Creators Update des App-Installers und der vorherigen Version des App-Installers:

| App-Installer, aktuelle Version | App-Installer, vorherige Version |
|------------------------------|----------------------------------|
| Der App-Installer zeigt App-Informationen an, bevor der Download gestartet wird | Der Browser fordert den Benutzer zum Herunterladen auf  |
| Der App-Installer führt den Download durch | Der Benutzer muss den Start des App-Pakets manuell initiieren |
| Nach dem Download des Pakets startet der App-Installer automatisch das App-Paket | Benutzer muss auf **installieren** klicken und das App-Paket manuell starten |
| Der App-Installer kümmert sich um die Beseitigung der heruntergeladenen Pakete | Der Benutzer muss die heruntergeladenen Dateien manuell löschen |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>Webinstallationserfahrung auf früheren Versionen von Windows 10
In Versionen vor dem Windows 10 Fall Creators Update kann der App-Installer nicht direkt eine App aus dem Internet installieren. Bei diesen Versionen kann der App-Installer nur App-Pakete installieren, die lokal verfügbar sind. Der App-Installer lädt stattdessen das Paket herunter und der Benutzer muss auf das zu installierende heruntergeladene Paket doppelklicken.


## <a name="microsoft-smartscreen-integration"></a>Microsoft SmartScreen-Integration

Microsoft SmartScreen war schon immer Teil des Installationsvorgangs für die Installation von Apps mithilfe des App-Installers. Durch SmartScreen wird sichergestellt, dass Benutzer vor schädlichen Inhalten geschützt sind, die sich auf ihren Geräten einschleichen können. Mit dem neuesten Update auf App-Installer ist die SmartScreen-Integration nahtloser und stabiler. Es werden Warnungen angezeigt, wenn Sie unbekannte Apps installieren und so das Geräte vor Schäden geschützt. 
