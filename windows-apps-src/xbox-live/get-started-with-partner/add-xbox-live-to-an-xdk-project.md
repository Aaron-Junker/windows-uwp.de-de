---
title: Hinzufügen von Xbox Live zu einem Projekt xdk-Version
description: Erfahren Sie, wie Xbox Live zu einem neuen oder vorhandenen Xbox Developer Kit (xdk-Version)-Projekt hinzufügen.
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, xdk-Version
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649045"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>Hinzufügen von Xbox Live zu einem neuen oder vorhandenen xdk-Version-Projekt

In diesem Thema wird beschrieben, wie Xbox Live zu einem neuen oder vorhandenen xdk-Version-Projekt hinzugefügt wird.

Der Prozess ist:

- Einrichten Ihrer Entwicklungsumgebung für Xbox One
- Ihre IDs erhalten
- Konfigurieren der Entwicklungskonsole
- Die Binärdatei der TitleID und SCID hinzugefügt


## <a name="setup-up-your-xbox-one-development-environment"></a>Einrichten Ihrer Entwicklungsumgebung für Xbox One
Richten Sie zuerst der Konsole, indem Sie folgenden Abschnitt von "Setting Up Your Xbox ein Development Environment" in der Dokumentation zu xdk-Version

## <a name="get-your-ids"></a>Ihre IDs erhalten

Um Xbox Live-Dienste zu aktivieren, müssen Sie mehrere IDs, die zum Konfigurieren des Development Kits und den Titel zu erhalten. Diese können mit dem gleichen Prozess erfolgen.

Sie werden Ihre IDs abrufen, indem Sie den Vorgang der [Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>Konfigurieren der Entwicklungskonsole

Sobald Sie Ihre IDs haben, folgen Sie der [konfigurieren Ihre entwicklungskonsole](configure-your-development-console.md) Anleitung für die entwicklungskonsole einrichten.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>Die Binärdatei der TitleID und SCID hinzugefügt
Während die Sandbox für die einzelnen Development Kits auf Plattformebene konfiguriert ist, sind die TitleID und SCID an eine bestimmte Binärdatei gebunden. Um die Binärdatei einer TitleID und SCID hinzuzufügen, ändern Sie die Datei "Package.appxmanifest" für diese Binärdatei durch Hinzufügen eines neuen Knotens in der <Extensions> Knoten wie folgt:

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

Weitere Informationen zu der Datei "appxmanifest.xml" finden Sie in-Projektvorlagen in Visual Studio für die ein Xbox-Entwicklung.

Finden Sie unter Application Manifest-Schema für eine Beschreibung der Anwendung Schema manifest.

**Das RequireXboxLive Flag** , wenn das Flag auf True, den Titel festgelegt ist RequireXboxLive nicht gestartet wird, es sei denn, Verbindungsebene Windows.Networking.Connectivity gibt als XboxLiveAccess zurück, und der Titel Authentifizierung mit Xbox Live löscht. Dadurch wird sichergestellt, dass der Titel der neuesten Inhaltsupdates stattgefunden hat. Wenn Konnektivität unterbrochen wird, während der Titel ausgeführt wird, wird der Titel angehalten.

Nur "Internet" Required"Titel sollte RequireXboxLive markieren Sie als" true ", und beachten Sie, die Markieren des Titels auf diese Weise nicht garantiert, dass die erforderlichen Dienste für den Titel sind und ausgeführt wird.
