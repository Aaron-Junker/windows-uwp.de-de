---
title: Verbundene Apps und Geräte (Projekt „Rome”)
description: In diesem Abschnitt wird beschrieben, wie Sie mithilfe der Remote Systems-Plattform Remotegeräte entdecken, eine App auf einem Remotegerät starten und mit einem App-Dienst auf einem Remotegerät kommunizieren.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP, verbundene Geräte, Remote Systeme, Rom, Project Rom
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 08004f22575be69fff3f8d8017ea34327d9a0b7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156034"
---
# <a name="connected-apps-and-devices-project-rome"></a>Verbundene Apps und Geräte (Projekt „Rome”)

In diesem Abschnitt wird erläutert, wie Sie Apps über Geräte und Plattformen mithilfe von [Project Rom](https://developer.microsoft.com/windows/project-rome)verbinden können. Um zu erfahren, wie Sie Project Rom in einem plattformübergreifenden Szenario implementieren, besuchen Sie die [Hauptseite der Dokumente für Project Rom](/windows/project-rome/).

Die meisten Benutzer verfügen über mehrere Geräte und beginnen häufig mit einer Aktivität auf einem Gerät und beenden Sie auf einem anderen Gerät. Dazu müssen Apps geräte- und plattformübergreifend sein. Project Rom ermöglicht Ihnen das Ermitteln von Remote Geräten, das Starten einer APP auf einem Remote Gerät und das kommunizieren mit einem App Service auf einem Remote Gerät.

Mit den in Windows 10, Version 1607, eingeführten [APIs für Remote Systeme](/uwp/api/Windows.System.RemoteSystems) können Sie apps schreiben, mit denen Benutzer eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät Fertigstellen können. Die Aufgabe bleibt der zentrale Fokus, und Benutzer können an dem für sie komfortabelsten Gerät arbeiten. Ein Benutzer könnte z. b. das Telefon auf dem Telefon im Auto abhören, aber wenn er zu Hause kommt, kann er die Wiedergabe auf seine Xbox One übertragen, die mit dem Stereo System Home verknüpft ist.

Sie können Project Rom auch für Begleit Geräte oder Remote Steuerungs Szenarios verwenden. Verwenden Sie die APP Service-Messaging-APIs, um einen app-Kanal zwischen zwei Geräten zum Senden und empfangen benutzerdefinierter Nachrichten zu erstellen. Beispielsweise können Sie eine APP für Ihr Telefon schreiben, mit der die Wiedergabe in Ihrem TV gesteuert wird, oder eine begleitende APP, die Informationen zu den Zeichen in einer TV-Show bereitstellt, die Sie durch eine andere APP überwachen.  

Geräte können über Bluetooth und drahtlos oder Remote über die Cloud verbunden werden. Sie werden durch die Microsoft-Konto (MSA) der Person verknüpft, die Sie verwendet.

Beispiele zum Ermitteln des Remote Systems, zum Starten einer APP auf einem Remote System und zum Senden von Nachrichten zwischen apps, die auf zwei Systemen ausgeführt werden, finden Sie unter [UWP-Beispiel für Remote Systeme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) .

Weitere Informationen zu Project Rom im Allgemeinen, einschließlich Ressourcen für die plattformübergreifende Integration, finden Sie unter [aka.MS/Project-Rome](https://developer.microsoft.com/windows/project-rome).

| Thema | BESCHREIBUNG |
|-------|-------------|
| [Starten einer App auf einem Remotegerät](launch-a-remote-app.md) | Erfahren Sie, wie Sie eine App auf einem Remotegerät starten. In diesem Thema werden der einfachste Anwendungsfall und die vorläufige Einrichtung behandelt.  |
| [Entdecken von Remotegeräten](discover-remote-devices.md)  | Erfahren Sie, wie Sie Geräte entdecken, zu denen Sie eine Verbindung herstellen können. |
| [Kommunizieren mit einem App-Remotedienst](communicate-with-a-remote-app-service.md) | Erfahren Sie, wie Sie mit einer App auf einem Remotegerät interagieren. |
| [Verbinden von Geräten über Remotesitzungen](remote-sessions.md) | Ermöglichen Sie die gemeinsame Nutzung auf verbundenen Geräten, indem Sie diese in einer Remotesitzung vereinen. |
| [Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)](useractivities.md)| Helfen Sie Benutzern dabei, das, was Sie in ihrer app ausgeführt haben, sogar über mehrere Geräte hinweg wieder aufzunehmen.|
| [Bewährte Methoden für Benutzeraktivitäten](useractivities-best-practices.md)| Informieren Sie sich über die empfohlenen Vorgehensweisen zum Erstellen und Aktualisieren von Benutzeraktivitäten.|