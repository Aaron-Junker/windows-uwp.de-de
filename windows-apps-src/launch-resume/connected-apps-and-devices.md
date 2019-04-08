---
title: Verbundene Apps und Geräte (Projekt „Rome”)
description: In diesem Abschnitt wird beschrieben, wie Sie mithilfe der Remote Systems-Plattform Remotegeräte entdecken, eine App auf einem Remotegerät starten und mit einem App-Dienst auf einem Remotegerät kommunizieren.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, Uwp, verbundene Geräte "," Remotesystemen "," ROM "," Projekt "ROME"
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: c785e6d2a8021148f572df88a6d9e6ba07c4a457
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601175"
---
# <a name="connected-apps-and-devices-project-rome"></a>Verbundene Apps und Geräte (Projekt „Rome”)

In diesem Abschnitt wird erläutert, wie zum Verbinden von apps für Geräte und Plattformen mit [Projekt "ROME"](https://developer.microsoft.com/en-us/windows/project-rome). Informationen zum Projekt "ROME" in einem plattformübergreifenden Szenario zu implementieren, finden Sie auf die [Haupt-Dokumentationsseite für Projekt "ROME"](https://docs.microsoft.com/en-us/windows/project-rome/).

Die meisten Benutzer verfügen über mehrere Geräte, wobei sie häufig eine Aktivität auf einem Gerät beginnen und auf einem anderen Gerät abschließen. Dazu müssen Apps geräte- und plattformübergreifend sein. Projekt "ROME" können Sie zum Ermitteln von Remotegeräten, starten Sie eine app auf einem Remotegerät und per app Service auf einem Remotegerät zu kommunizieren.

Die mit Windows 10, Version 1607, eingeführten [Remotesysteme-APIs](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems) ermöglichen Ihnen das Schreiben von Apps, mit denen Benutzer eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät abschließen können. Die Aufgabe bleibt der zentrale Fokus, und Benutzer können an dem für sie komfortabelsten Gerät arbeiten. Zum Beispiel hört ein Benutzer vielleicht Radio auf seinem Mobiltelefon. Aber zu Hause angekommen möchte er möglicherweise die Wiedergabe auf seine Xbox One übertragen, die in die Heim-Stereoanlage eingebunden ist.

Sie können Project Rome auch für Begleitgeräte oder Remotesteuerungsszenarien verwenden. Verwenden Sie die App-Dienst-Messaging-APIs, um einen App-Kanal zwischen zwei Geräten zum Senden und Empfangen von benutzerdefinierten Nachrichten zu erstellen. Beispielsweise können Sie eine App für Ihr Mobiltelefon schreiben, die die Wiedergabe auf Ihrem Fernsehgerät steuert, oder eine Begleit-App, die Informationen über die Charaktere in einer Fernsehsendung bereitstellt, die Sie sich in einer anderen App ansehen.  

Geräte können über Bluetooth und Wireless LAN proximal oder remote über die Cloud verbunden werden und sind über das Microsoft-Konto (MSA) der Person verbunden, die sie verwendet.

Unter [UWP-Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) finden Sie Beispiele für die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.

Weitere Informationen zu Project Rome im Allgemeinen, einschließlich Ressourcen für die plattformübergreifende Integration, finden Sie unter [aka.ms/project-rome](https://aka.ms/project-rome).

| Thema | Beschreibung |
|-------|-------------|
| [Starten einer App auf einem Remotegerät](launch-a-remote-app.md) | Erfahren Sie, wie Sie eine App auf einem Remotegerät starten. In diesem Thema werden der einfachste Anwendungsfall und die vorläufige Einrichtung behandelt.  |
| [Entdecken von Remotegeräten](discover-remote-devices.md)  | Erfahren Sie, wie Sie Geräte entdecken, zu denen Sie eine Verbindung herstellen können. |
| [Kommunizieren mit einem App-Remotedienst](communicate-with-a-remote-app-service.md) | Erfahren Sie, wie Sie mit einer App auf einem Remotegerät interagieren. |
| [Verbinden von Geräten über Remotesitzungen](remote-sessions.md) | Ermöglichen Sie die gemeinsame Nutzung auf verbundenen Geräten, indem Sie diese in einer Remotesitzung vereinen. |
| [Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)](useractivities.md)| Helfen Sie Benutzern, die fortgesetzt, wo sie auch auf mehreren Geräten in Ihrer app aufgehört haben.|
| [Bewährte Methoden für Benutzer Aktivitäten](useractivities-best-practices.md)| Erfahren Sie, die empfohlenen Methoden zum Erstellen und Aktualisieren von Benutzeraktivitäten.|
